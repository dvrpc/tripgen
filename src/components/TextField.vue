<script setup>
const model = defineModel();
const props = defineProps({
  placeholder: String,
  name: String,
  required: Boolean,
});
</script>

<template>
  <input
    class="pvm-search-control-input"
    type="text"
    v-model="model"
    :name="name"
    :placeholder="placeholder"
    required
  />
</template>
<style>
.pvm-search-control-input {
  display: inline-block;
  border: 0;
  padding: 15px;
  font-family: Montserrat, Tahoma, sans-serif;
  font-size: 16px;
  width: 100% !important;
  height: 50px;
  border-style: solid;
  border-color: #0f4d90;
  border-width: 2px;
  outline: none;
  margin-top: 4px;
  margin-bottom: 4px;
}
</style>
