<script>
export default {
  props: {
    placeholder: String,
  },
};
</script>
<template>
  <input class="pvm-search-control-input" :placeholder="placeholder" />
</template>
<style>
.pvm-search-control-input {
  display: inline-block;
  border: 0;
  padding: 15px;
  font-family: Montserrat, Tahoma, sans-serif;
  font-size: 16px;
  width: 250px;
  height: 50px;
  border-style: solid;
  border-color: #0f4d90;
  border-width: 2px;
  outline: none;
}
</style>
