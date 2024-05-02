<script setup>
import { nextTick, ref, watch } from "vue";
import TextField from "@/components/TextField.vue";
import Button from "@/components/Button.vue";
import debounce from "@/utilities/debounce";
import { store } from "@/utilities/store";
import DataViz from "@/components/DataViz.vue";

const title = ref("");
const address = ref("");
const items = ref([]);
const vizKey = ref(0);

const formSubmit = async (e) => {
  e.preventDefault();
  var data = new FormData(e.target);
  store.formData = Object.fromEntries(data);
  vizKey.value += 1;
};

watch(
  address,
  debounce(async (val) => {
    if (val) {
      const res = await fetch(
        `https://services.arcgis.com/fLeGjb7u4uXqeF9q/arcgis/rest/services/DOR_Parcel/FeatureServer/0/query?where=ADDR_SOURCE+LIKE+%27%25${encodeURI(
          val
        )}%25%27&objectIds=&time=&geometry=&geometryType=esriGeometryEnvelope&inSR=&spatialRel=esriSpatialRelEnvelopeIntersects&resultType=none&distance=0.0&units=esriSRUnit_Meter&relationParam=&returnGeodetic=false&outFields=*&returnGeometry=true&returnCentroid=false&returnEnvelope=false&featureEncoding=esriDefault&multipatchOption=xyFootprint&maxAllowableOffset=&geometryPrecision=&outSR=&defaultSR=&datumTransformation=&applyVCSProjection=false&returnIdsOnly=false&returnUniqueIdsOnly=false&returnCountOnly=false&returnExtentOnly=false&returnQueryGeometry=false&returnDistinctValues=false&cacheHint=false&orderByFields=&groupByFieldsForStatistics=&outStatistics=&having=&resultOffset=&resultRecordCount=&returnZ=false&returnM=false&returnExceededLimitFeatures=true&quantizationParameters=&sqlFormat=none&f=pgeojson&token=`
      );
      const data = await res.json();
      items.value = data.features;
    } else {
      store.selectedFeature = {};
      items.value = [];
    }
  }, 1000)
);

watch(store, (value) => {
  address.value = value.selectedFeature.properties.ADDR_STD;
});

const onClick = (item) => {
  store.selectedFeature = item;
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
    <template v-if="!Object.keys(store.selectedFeature).length">
      <div id="suggestions">
        <div v-for="item in items" :key="item.id" style="padding: 0">
          <button type="button" @click="onClick(item)">
            {{ item.properties.ADDR_STD }}
          </button>
        </div>
      </div>
    </template>
    <template v-if="!!Object.keys(store.selectedFeature).length">
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
  <template v-if="!!Object.keys(store.formData).length">
    <DataViz :key="vizKey" />
  </template>
</template>

<style>
label {
  display: inline-block;
}

#suggestions > div > button {
  cursor: pointer;
}
</style>