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
