<script setup>
import { defineProps } from "vue";
import { Bar } from "vue-chartjs";
import {
  Chart as ChartJS,
  Title,
  Tooltip,
  BarElement,
  CategoryScale,
  LinearScale,
} from "chart.js";
import { store } from "@/utilities/store";

ChartJS.register(Title, Tooltip, BarElement, CategoryScale, LinearScale);
const { employees, residents } = store.formData;

const tripsAM = 4.229 + 2.072 * employees + 0.139 * residents;
const tripsPM = -2.242 + 3.817 * employees + 0.181 * residents;

const amChartData = {
  labels: ["Person Trips", "Residents", "Employees"],
  datasets: [{ data: [tripsAM, residents, employees] }],
};

const amChartOptions = {
  responsive: true,
  indexAxis: "y",
  plugins: {
    title: {
      display: true,
      text: "Person Trips AM",
    },
  },
};

const pmChartData = {
  labels: ["Person Trips", "Residents", "Employees"],
  datasets: [{ data: [tripsPM, residents, employees] }],
};

const pmChartOptions = {
  responsive: true,
  indexAxis: "y",
  plugins: {
    title: {
      display: true,
      text: "Person Trips PM",
    },
  },
};
</script>

<template>
  <Bar id="chartAM" :options="amChartOptions" :data="amChartData"></Bar>
  <Bar id="chartPM" :options="pmChartOptions" :data="pmChartData"></Bar>
</template>

<style></style>
