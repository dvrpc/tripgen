<script setup>
import Map from "./components/Map.vue";
import SideBar from "./components/SideBar.vue";

const location = {
  lng: -71.224518,
  lat: 42.213995,
  bearing: 0,
  pitch: 0,
  zoom: 10,
};
</script>

<template>
  <header>
    <div id="branding">
      <img id="dvrpc-logo" src="./assets/dvrpc.svg" />
      <hr />
      <img id="philly-logo" src="./assets/city-of-philadelphia.svg" />
    </div>
    <div id="application-title">
      <h2>Philadelphia Trip Generation</h2>
    </div>
  </header>
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
  height: 100%;
}

#sidebar {
  color: black;
  background-color: white;
  width: 28vw;
  max-height: 100vh;
  padding-left: 2rem;
  padding-right: 2rem;
  padding-top: 1rem;
  padding-bottom: 1rem;
  overflow-y: scroll;
}

.map-container {
  flex: 1;
}

#branding {
  display: flex;
  flex-direction: row;
  align-items: center;
  background-color: #2176d2;
  height: 6vh;
  width: 100%;
  padding-right: 2rem;
  padding-left: 2rem;
}

#application-title {
  color: white;
  display: inline-block;
  background-color: #0f4d90;
  width: 100%;
  padding-right: 2rem;
  padding-left: 2rem;
}

#dvrpc-logo {
  height: 2rem;
  margin-top: 0.5rem;
}

#philly-logo {
  height: 4rem;
}

#branding > hr {
  height: 60%;
  margin-left: 1rem;
  margin-right: 0.4rem;
  color: white;
}
</style>
