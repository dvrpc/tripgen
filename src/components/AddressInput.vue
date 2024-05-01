<script setup>
import TextField from "./TextField.vue";

defineProps(["modelValue"]);
defineEmits(["update:modelValue"]);
</script>

<template>
  <label>
    Project Address
    <TextField
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
      placeholder="Address"
      name="address"
      required
    ></TextField>
  </label>
</template>

<style>
#suggestions {
  max-height: 20vh;
  overflow-y: scroll;
  width: 100%;
}

#suggestions > div {
  text-align: left;
  width: 100%;
  border-bottom: 1px solid;
}

#suggestions > div > button {
  cursor: pointer;
}
</style>
