<!-- components/SensorChart.vue -->
<template>
  <div class="w-full max-w-4xl mx-auto">
    <canvas ref="chartCanvas"></canvas>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import {
  Chart,
  LineController,
  LineElement,
  PointElement,
  LinearScale,
  Title,
  CategoryScale,
  Legend,
  Tooltip,
} from "chart.js";

Chart.register(
  LineController,
  LineElement,
  PointElement,
  LinearScale,
  Title,
  CategoryScale,
  Legend,
  Tooltip
);

const chartCanvas = ref(null);
// eslint-disable-next-line no-unused-vars
let chartInstance = null;

const fetchAndRender = async () => {
  const responseProcessed = await fetch(
    "http://127.0.0.1:8000/sensor/processed",
    {
      method: "GET",
      headers: {
        "Content-Type": "application/json",
      },
    }
  );

  if (!responseProcessed.ok) throw new Error("Failed to fetch");

  const data = await responseProcessed.json();

  const labels = data.map((d) => new Date(d.timestamp).toLocaleString());
  const temperature = data.map((d) => d.temperature);
  const humidity = data.map((d) => d.humidity);
  const airQuality = data.map((d) => d.air_quality);

  chartInstance = new Chart(chartCanvas.value, {
    type: "line",
    data: {
      labels,
      datasets: [
        {
          label: "Temperature (°C)",
          data: temperature,
          borderColor: "rgba(255, 99, 132, 1)",
          backgroundColor: "rgba(255, 99, 132, 0.2)",
          tension: 0.4,
        },
        {
          label: "Humidity (%)",
          data: humidity,
          borderColor: "rgba(54, 162, 235, 1)",
          backgroundColor: "rgba(54, 162, 235, 0.2)",
          tension: 0.4,
        },
        {
          label: "Air Quality",
          data: airQuality,
          borderColor: "rgba(255, 206, 86, 1)",
          backgroundColor: "rgba(255, 206, 86, 0.2)",
          tension: 0.4,
        },
      ],
    },
    options: {
      responsive: true,
      plugins: {
        legend: {
          position: "top",
        },
        title: {
          display: true,
          text: "Sensor Readings Over Time",
        },
      },
    },
  });
};

onMounted(fetchAndRender);
</script>

<style scoped>
canvas {
  max-width: 100%;
}
</style>
