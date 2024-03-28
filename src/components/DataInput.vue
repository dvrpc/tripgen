<script setup>
import { ref } from "vue";
import TextField from "./TextField.vue";
import Button from "./Button.vue";
import router from "@/router";

const title = ref("");

const formSubmit = (e) => {
  e.preventDefault();
  var data = new FormData(e.target);
  router.push({ name: "report", query: { ...Object.fromEntries(data) } });
};
</script>

<template>
  <h2>{{ title ? title : "Create Scenario" }}</h2>
  <form @submit="formSubmit">
    <label
      >Project Title
      <TextField v-model="title" placeholder="Title" name="title"></TextField>
    </label>
    <label
      >Project Address
      <TextField placeholder="Address" name="address"></TextField>
    </label>
    <label
      >Number of Residents
      <TextField placeholder="Residents" name="residents"></TextField>
    </label>
    <label
      >Number of Employees
      <TextField placeholder="Employees" name="employees"></TextField>
    </label>
    <Button text="Generate Report"></Button>
  </form>
</template>

<style>
label {
  display: inline-block;
}
</style>
