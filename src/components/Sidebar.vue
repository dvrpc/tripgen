<script>
import TextField from "./TextField.vue";
import Button from "./Button.vue";

export default {
  components: {
    TextField,
    Button,
  },
};
</script>

<template>
  <div id="sidebar">
    <form>
      <label
        >Project Title
        <TextField></TextField>
      </label>
      <label
        >Project Address
        <TextField></TextField>
      </label>
      <label
        >Number of Residents
        <TextField></TextField>
      </label>
      <label
        >Number of Employees
        <TextField></TextField>
      </label>
      <Button text="Generate Report"></Button>
    </form>
  </div>
</template>

<style>
#sidebar {
  color: black;
  background-color: white;
  width: 25vw;
  height: 100vh;
}

label {
  display: inline-block;
}
</style>
