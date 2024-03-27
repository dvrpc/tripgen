<script>
export default {
  props: { text: String },
};
</script>
<template>
  <button class="pvm-search-control-button">{{ text }}</button>
</template>
<style>
.pvm-search-control-button {
  display: inline-block;
  color: #fff;
  background: #0f4d90;
  margin-right: 1.5px;
  height: 50px;
}

button {
  padding: 0;
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
  border: 0;
  border-radius: 0;
  background: transparent;
  line-height: 1;
  cursor: auto;
}

button:focus {
  outline: none;
}
</style>
