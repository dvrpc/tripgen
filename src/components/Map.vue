<script>
import { reactive, watch } from "vue";
import mapboxgl from "mapbox-gl";
import { LngLatBounds } from "mapbox-gl";
import "mapbox-gl/dist/mapbox-gl.css";
import { store } from "@/utilities/store";
import { getBoundingBox } from "@/utilities";

mapboxgl.accessToken =
  "pk.eyJ1IjoibW1vbHRhIiwiYSI6ImNqZDBkMDZhYjJ6YzczNHJ4cno5eTcydnMifQ.RJNJ7s7hBfrJITOBZBdcOA";

export default {
  props: ["modelValue"],

  data: () => ({ map: null }),

  mounted() {
    const { lng, lat, zoom, bearing, pitch } = this.modelValue;
    const maxExtent = new LngLatBounds([
      [-76.09405517578125, 39.49211914385648],
      [-74.32525634765625, 40.614734298694216],
    ]);

    const map = new mapboxgl.Map({
      container: this.$refs.mapContainer,
      style: "mapbox://styles/mapbox/light-v11",
      center: [-75.2273, 40.071],
      bearing,
      pitch,
      zoom,
    });

    store.map = map;

    map.on("load", () => {
      map.addSource("parcels", {
        type: "geojson",
        data: { type: "FeatureCollection", features: [] },
      });
      map.addLayer({
        id: "parcels-fill",
        type: "fill",
        source: "parcels",
        layout: {},
        paint: {
          "fill-color": "#1868d1",
          "fill-opacity": [
            "case",
            ["boolean", ["feature-state", "click"], false],
            0.8,
            0.2,
          ],
        },
      });
    });

    map.on("click", "parcels-fill", (event) => {
      store.selectedFeature = event.features[0];
      map.removeFeatureState({ source: "parcels" });
      map.setFeatureState(
        {
          source: "parcels",
          id: store.selectedFeature.id,
        },
        { click: true }
      );
      store.formData = {};
    });

    this.map = map;

    watch(
      () => store.selectedFeature,
      (value) => {
        if (!!Object.keys(value).length) {
          map.getSource("parcels").setData(
            `https://services.arcgis.com/fLeGjb7u4uXqeF9q/ArcGIS/rest/services/DOR_Parcel/FeatureServer/0/query?where=1%3D1&objectIds=&time=&geometry=${encodeURI(
              JSON.stringify({
                rings: [...value.geometry.coordinates],
                spatialReference: {
                  wkid: 4326,
                },
              })
            )}&geometryType=esriGeometryPolygon&inSR=&spatialRel=esriSpatialRelEnvelopeIntersects&resultType=none&distance=&units=esriSRUnit_Meter&relationParam=&returnGeodetic=false&outFields=*&returnGeometry=true&returnCentroid=false&returnEnvelope=false&featureEncoding=esriDefault&multipatchOption=xyFootprint&maxAllowableOffset=&geometryPrecision=&outSR=&defaultSR=&datumTransformation=&applyVCSProjection=false&returnIdsOnly=false&returnUniqueIdsOnly=false&returnCountOnly=false&returnExtentOnly=false&returnQueryGeometry=false&returnDistinctValues=false&cacheHint=false&orderByFields=&groupByFieldsForStatistics=&outStatistics=&having=&resultOffset=&resultRecordCount=&returnZ=false&returnM=false&returnExceededLimitFeatures=true&quantizationParameters=&sqlFormat=none&f=pgeojson&token=`
          );

          const bbox = getBoundingBox(value);
          const { xMin, xMax, yMin, yMax } = bbox;
          xMin &&
            map.fitBounds(
              [
                [xMin, yMin],
                [xMax, yMax],
              ],
              {
                maxZoom: 17,
              }
            );
        }
      }
    );
  },

  unmounted() {
    this.map.remove();
    this.map = null;
  },
};
</script>

<template>
  <div ref="mapContainer" class="map-container"></div>
</template>

<style>
.map-container {
  flex: 1;
}
</style>
