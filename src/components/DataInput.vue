<script setup>
import { ref, watch } from "vue";
import TextField from "./TextField.vue";
import Button from "./Button.vue";
import router from "@/router";
import debounce from "@/utilities/debounce";

let selectedAddress = null;
const title = ref("");
const address = ref("");
const items = ref([]);
const isSelected = ref(false);

const formSubmit = (e) => {
  e.preventDefault();
  var data = new FormData(e.target);
  router.push({ name: "report", query: { ...Object.fromEntries(data) } });
};

watch(
  address,
  debounce(async (val) => {
    if (val) {
      const res = await fetch(
        `https://services.arcgis.com/fLeGjb7u4uXqeF9q/arcgis/rest/services/DOR_Parcel/FeatureServer/0/query?where=ADDR_SOURCE+LIKE+%27%25${encodeURI(
          val
        )}%25%27&objectIds=&time=&geometry=&geometryType=esriGeometryEnvelope&inSR=&spatialRel=esriSpatialRelIntersects&resultType=none&distance=0.0&units=esriSRUnit_Meter&relationParam=&returnGeodetic=false&outFields=*&returnGeometry=true&returnCentroid=false&returnEnvelope=false&featureEncoding=esriDefault&multipatchOption=xyFootprint&maxAllowableOffset=&geometryPrecision=&outSR=&defaultSR=&datumTransformation=&applyVCSProjection=false&returnIdsOnly=false&returnUniqueIdsOnly=false&returnCountOnly=false&returnExtentOnly=false&returnQueryGeometry=false&returnDistinctValues=false&cacheHint=false&orderByFields=&groupByFieldsForStatistics=&outStatistics=&having=&resultOffset=&resultRecordCount=&returnZ=false&returnM=false&returnExceededLimitFeatures=true&quantizationParameters=&sqlFormat=none&f=pgeojson&token=`
      );
      const data = await res.json();
      items.value = data.features;
    } else {
      selectedAddress = null;
      isSelected.value = false;
      items.value = [];
    }
  }, 1000)
);

const onClick = (item) => {
  address.value = item.properties.ADDR_STD;
  selectedAddress = item;
  isSelected.value = true;
};
</script>

<template>
  <h2>{{ title ? title : "Create Scenario" }}</h2>
  <form @submit="formSubmit">
    <label
      >Project Title
      <TextField
        v-model="title"
        placeholder="Title"
        name="title"
        required
      ></TextField>
    </label>
    <label
      >Project Address
      <TextField
        v-model="address"
        placeholder="Address"
        name="address"
        required
      ></TextField>
    </label>
    <template v-if="!isSelected">
      <div>
        <div id="suggestions">
          <div v-for="item in items" :key="item.id" style="padding: 0">
            <button type="button" @click="onClick(item)">
              {{ item.properties.ADDR_STD }}
            </button>
          </div>
        </div>
      </div>
    </template>
    <template v-if="isSelected">
      <label
        >Number of Residents
        <TextField
          placeholder="Residents"
          name="residents"
          required
        ></TextField>
      </label>
      <label
        >Number of Employees
        <TextField
          placeholder="Employees"
          name="employees"
          required
        ></TextField>
      </label>
      <Button text="Generate Report"></Button>
    </template>
  </form>
</template>

<style>
label {
  display: inline-block;
}
</style>
