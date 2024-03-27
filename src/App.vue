<script>
import Map from "./components/Map.vue";
import "../node_modules/mapbox-gl/dist/mapbox-gl.css";
import Sidebar from "./components/Sidebar.vue";

export default {
  components: {
    Map,
    Sidebar,
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
    <Sidebar></Sidebar>
    <Map v-model="location"></Map>
  </div>
</template>

<style>
#layout {
  flex: 1;
  display: flex;
}
</style>
