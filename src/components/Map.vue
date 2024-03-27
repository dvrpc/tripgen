<template>
  <div ref="mapContainer" class="map-container"></div>
</template>

<script>
import mapboxgl from "mapbox-gl";
import { LngLatBounds } from "mapbox-gl";
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

    map.on("load", () => {
      map.addSource("parcel", {
        type: "geojson",
        data: "https://services.arcgis.com/fLeGjb7u4uXqeF9q/arcgis/rest/services/DOR_Parcel/FeatureServer/0/query?where=1%3D1&objectIds=&time=&geometry=&geometryType=esriGeometryEnvelope&inSR=&spatialRel=esriSpatialRelIntersects&resultType=none&distance=0.0&units=esriSRUnit_Meter&relationParam=&returnGeodetic=false&outFields=*&returnGeometry=true&returnCentroid=false&returnEnvelope=false&featureEncoding=esriDefault&multipatchOption=xyFootprint&maxAllowableOffset=&geometryPrecision=&outSR=&defaultSR=&datumTransformation=&applyVCSProjection=false&returnIdsOnly=false&returnUniqueIdsOnly=false&returnCountOnly=false&returnExtentOnly=false&returnQueryGeometry=false&returnDistinctValues=false&cacheHint=false&orderByFields=&groupByFieldsForStatistics=&outStatistics=&having=&resultOffset=&resultRecordCount=&returnZ=false&returnM=false&returnExceededLimitFeatures=true&quantizationParameters=&sqlFormat=none&f=pgeojson&token=",
      });

      map.addLayer({
        id: "parcel-layer",
        type: "fill",
        source: "parcel",
      });
    });

    this.map = map;
  },

  unmounted() {
    this.map.remove();
    this.map = null;
  },
};
</script>

<style>
.map-container {
  flex: 1;
}
</style>
