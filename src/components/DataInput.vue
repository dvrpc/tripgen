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
      <TextField
        v-model="title"
        placeholder="Title"
        name="title"
        required
      ></TextField>
    </label>
    <label
      >Project Address
      <TextField placeholder="Address" name="address" required></TextField>
    </label>
    <label
      >Number of Residents
      <TextField placeholder="Residents" name="residents" required></TextField>
    </label>
    <label
      >Number of Employees
      <TextField placeholder="Employees" name="employees" required></TextField>
    </label>
    <Button text="Generate Report"></Button>
  </form>
</template>

<style>
label {
  display: inline-block;
}
</style>
