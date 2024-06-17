<script setup>
import { Bar } from "vue-chartjs";
import {
  Chart as ChartJS,
  Colors,
  Title,
  Tooltip,
  BarElement,
  CategoryScale,
  LinearScale,
} from "chart.js";
import { store } from "@/utilities/store";
import { jsPDF } from "jspdf";
import ChartDataLabels from "chartjs-plugin-datalabels";

ChartJS.register(
  Title,
  Tooltip,
  BarElement,
  CategoryScale,
  LinearScale,
  Colors,
  ChartDataLabels
);
const { employees, residents } = store.formData;

const tripsAM = 2.2901 + 0.1441 * residents;
const tripsPM = 5.207 + 0.1818 * residents;

const plugin = {
  id: "customCanvasBackgroundColor",
  beforeDraw: (chart, args, options) => {
    const { ctx } = chart;
    ctx.save();
    ctx.globalCompositeOperation = "destination-over";
    ctx.fillStyle = "white";
    ctx.fillRect(0, 0, chart.width, chart.height);
    ctx.restore();
  },
};

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
    datalabels: {
      color: "white",
      display: function (context) {
        return context.dataset.data[context.dataIndex] > 15;
      },
      font: {
        weight: "bold",
      },
      formatter: Math.round,
    },
    aspectRatio: 5 / 3,
    layout: {
      padding: {
        top: 24,
        right: 16,
        bottom: 0,
        left: 8,
      },
    },
    elements: {
      line: {
        fill: false,
      },
      point: {
        hoverRadius: 7,
        radius: 5,
      },
    },
    scales: {
      x: {
        stacked: true,
      },
      y: {
        stacked: true,
      },
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
    datalabels: {
      color: "white",
      display: function (context) {
        return context.dataset.data[context.dataIndex] > 15;
      },
      font: {
        weight: "bold",
      },
      formatter: Math.round,
    },
    aspectRatio: 5 / 3,
    layout: {
      padding: {
        top: 24,
        right: 16,
        bottom: 0,
        left: 8,
      },
    },
    elements: {
      line: {
        fill: false,
      },
      point: {
        hoverRadius: 7,
        radius: 5,
      },
    },
    scales: {
      x: {
        stacked: true,
      },
      y: {
        stacked: true,
      },
    },
  },
};

const downloadClick = () => {
  const chartAM = document.getElementById("chartAM");
  const chartAMImage = chartAM.toDataURL("image/jpeg", 1.0);
  const chartPM = document.getElementById("chartPM");
  const chartPMImage = chartPM.toDataURL("image/jpeg", 1.0);
  let pdf = new jsPDF({
    orientation: "landscape",
  });
  pdf.addImage(chartAMImage, "jpeg", 15, 15, 140, 75);
  pdf.addImage(chartPMImage, "jpeg", 15, 100, 140, 75);
  pdf.save("viz.pdf");
};
</script>

<template>
  <hr style="margin-top: 1rem; margin-bottom: 1rem" />
  <p style="text-transform: uppercase">
    {{ store.formData.address }}
  </p>
  <p>
    <span style="margin-right: 0.75rem"
      >Employees: {{ store.formData.employees }}</span
    >
    <span>Residents: {{ store.formData.residents }}</span>
  </p>
  <Bar
    id="chartAM"
    :options="amChartOptions"
    :data="amChartData"
    :plugins="[plugin]"
  ></Bar>
  <Bar
    id="chartPM"
    :options="pmChartOptions"
    :data="pmChartData"
    :plugins="[plugin]"
  ></Bar>
  <button id="download" :onclick="downloadClick">
    Export PDF
    <span>
      <svg
        width="24px"
        height="24px"
        viewBox="0 0 24 24"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
      >
        <path
          d="M12 16L12 8"
          stroke="#323232"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
        />
        <path
          d="M9 13L11.913 15.913V15.913C11.961 15.961 12.039 15.961 12.087 15.913V15.913L15 13"
          stroke="#323232"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
        />
        <path
          d="M3 15L3 16L3 19C3 20.1046 3.89543 21 5 21L19 21C20.1046 21 21 20.1046 21 19L21 16L21 15"
          stroke="#323232"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
        />
      </svg>
    </span>
  </button>
</template>

<style>
#download {
  margin-left: auto;
  background: none;
  color: inherit;
  border: none;
  padding: 0;
  font: inherit;
  cursor: pointer;
  outline: inherit;
  display: flex;
  align-items: center;
}

#download > span {
  margin-left: 0.5rem;
}
</style>
