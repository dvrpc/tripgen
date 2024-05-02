<script setup>
import Map from "./components/Map.vue";
import SideBar from "./components/SideBar.vue";

const location = {
  lng: -71.224518,
  lat: 42.213995,
  bearing: 0,
  pitch: 0,
  zoom: 9,
};
</script>

<template>
  <div id="layout">
    <div id="sidebar"><SideBar></SideBar></div>
    <Map v-model="location"></Map>
  </div>
</template>

<style>
#layout {
  flex: 1;
  display: flex;
  width: 100%;
}

#sidebar {
  color: black;
  background-color: white;
  width: 25vw;
  max-height: 100vh;
  padding-left: 1rem;
  padding-right: 1rem;
  padding-top: 1rem;
  padding-bottom: 1rem;
  overflow-y: scroll;
}

.map-container {
  flex: 1;
}
</style>
