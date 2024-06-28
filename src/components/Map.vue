<script>
import { reactive, watch } from "vue";
import mapboxgl from "mapbox-gl";
import { LngLatBounds } from "mapbox-gl";
import "mapbox-gl/dist/mapbox-gl.css";
import { store } from "@/utilities/store";
import { getBoundingBox } from "@/utilities";

mapboxgl.accessToken =
  "pk.eyJ1IjoiY3J2YW5wb2xsYXJkIiwiYSI6ImNseHVpZmprazI4bWoycXB2MTljMWF1YjUifQ.jLMaSXqIUV5N2IxYlk5ZiQ";

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
      center: [-75.146484, 39.985275],
      bearing,
      pitch,
      zoom,
    });

    store.map = map;

    map.on("load", () => {
      map.addSource("parcel", {
        type: "geojson",
        data: { type: "FeatureCollection", features: [] },
      });
      map.addSource("county", {
        type: "geojson",
        data: "https://arcgis.dvrpc.org/portal/rest/services/Boundaries/CountyBoundaries_Line/FeatureServer/0/query?where=co_name+like+%27%25Philadelphia%25%27&objectIds=&time=&geometry=&geometryType=esriGeometryEnvelope&inSR=&spatialRel=esriSpatialRelIntersects&distance=&units=esriSRUnit_Foot&relationParam=&outFields=*&returnGeometry=true&maxAllowableOffset=&geometryPrecision=&outSR=&havingClause=&gdbVersion=&historicMoment=&returnDistinctValues=false&returnIdsOnly=false&returnCountOnly=false&returnExtentOnly=false&orderByFields=&groupByFieldsForStatistics=&outStatistics=&returnZ=false&returnM=false&multipatchOption=xyFootprint&resultOffset=&resultRecordCount=&returnTrueCurves=false&returnExceededLimitFeatures=false&quantizationParameters=&returnCentroid=false&timeReferenceUnknownClient=false&sqlFormat=none&resultType=&featureEncoding=esriDefault&datumTransformation=&f=geojson",
      });

      map.addSource("parcels", {
        type: "geojson",
        data: { type: "FeatureCollection", features: [] },
      });
      map.addLayer({
        id: "county",
        type: "line",
        source: "county",
        paint: {
          "line-color": "#6e6e6e",
          "line-width": 2,
        },
      });
      map.addLayer({
        id: "parcel-selected",
        type: "fill-extrusion",
        source: "parcel",
        layout: {},
        paint: {
          "fill-extrusion-color": "#1868d1",
          "fill-extrusion-height": 50,
        },
      });
      map.addLayer({
        id: "parcels-outline",
        type: "line",
        source: "parcels",
        layout: {},
        paint: {
          "line-color": "#aaa",
          "line-width": 1.75,
        },
      });
    });

    map.on("click", "parcels-outline", (event) => {
      store.selectedFeature = event.features[0];
      store.map.getSource("parcel").setData(store.selectedFeature);
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
                pitch: 25,
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
