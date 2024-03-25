<script>
import Map from "./components/Map.vue";
import "../node_modules/mapbox-gl/dist/mapbox-gl.css";

export default {
  components: {
    Map,
  },

  data: () => ({
    location: {
      lng: -71.224518,
      lat: 42.213995,
      bearing: 0,
      pitch: 0,
      zoom: 9,
    },
  }),
};
</script>

<template>
  <div id="layout">
    <Map v-model="location" />
  </div>
</template>

<style>
#layout {
  flex: 1;
  display: flex;
}

#sidebar {
  background-color: rgb(35 55 75 / 90%);
  color: #fff;
  padding: 6px 12px;
  font-family: monospace;
  z-index: 1;
  position: absolute;
  top: 0;
  left: 0;
  margin: 12px;
  border-radius: 4px;
}
</style>
