<script>
export default {
  props: { text: String },
};
</script>
<template>
  <button class="btn">{{ text }}</button>
</template>
<style>
.btn {
  display: inline-block;
  color: #fff;
  background: #0f4d90;
  margin-right: 1.5px;
  margin-top: 4px;
  margin-bottom: 4px;
  padding: 0;
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
  border: 0;
  border-radius: 0;
  line-height: 1;
  font-family: "Montserrat";
  letter-spacing: 0.03rem;
  height: 45px;
  width: 100%;
  text-transform: uppercase;
  cursor: pointer;
}

button:focus {
  outline: none;
}
</style>
