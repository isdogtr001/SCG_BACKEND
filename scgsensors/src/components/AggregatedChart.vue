<template>
  <div class="p-4">
    <h2 class="text-2xl font-bold mb-4">Aggregated Sensor Stats</h2>

    <div v-if="loading">Loading chart data...</div>
    <div v-else-if="error" class="text-red-500">{{ error }}</div>

    <!-- Always render the canvas -->
    <canvas ref="canvasEl" v-show="!loading && !error"></canvas>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import {
  Chart,
  BarController,
  BarElement,
  CategoryScale,
  LinearScale,
  Title,
  Tooltip,
  Legend,
} from "chart.js";

// Register Chart.js components
Chart.register(
  BarController,
  BarElement,
  CategoryScale,
  LinearScale,
  Title,
  Tooltip,
  Legend
);

const canvasEl = ref(null);
const loading = ref(true);
const error = ref(null);
let chartInstance = null;

onMounted(async () => {
  try {
    const response = await fetch("http://127.0.0.1:8000/sensor/aggregated");
    if (!response.ok) throw new Error(`Failed to fetch: ${response.status}`);
    const data = await response.json();

    console.log(data);

    const labels = ["Temperature", "Humidity", "Air Quality"];

    const datasets = [
      {
        label: "Mean",
        backgroundColor: "#3b82f6",
        data: [
          data.temperature.mean,
          data.humidity.mean,
          data.air_quality.mean,
        ],
      },
      {
        label: "Median",
        backgroundColor: "#10b981",
        data: [
          data.temperature.median,
          data.humidity.median,
          data.air_quality.median,
        ],
      },
      {
        label: "Min",
        backgroundColor: "#f59e0b",
        data: [data.temperature.min, data.humidity.min, data.air_quality.min],
      },
      {
        label: "Max",
        backgroundColor: "#ef4444",
        data: [data.temperature.max, data.humidity.max, data.air_quality.max],
      },
    ];

    console.log("epwijfpwejfijioewfjoijiowedjiojeiowjif");

    // Destroy old chart if it exists
    if (chartInstance) chartInstance.destroy();

    console.log("epwijfpwejfijioewfjoijiowedjiojeiowji222222");

    chartInstance = new Chart(canvasEl.value, {
      type: "bar",
      data: { labels, datasets },
      options: {
        responsive: true,
        plugins: {
          legend: { position: "top" },
          title: {
            display: true,
            text: "Sensor Stats (Mean, Median, Min, Max)",
          },
        },
      },
    });
    console.log("epwijfpwejfijioewfjoijiowedjiojeiow3333333333333333");
  } catch (err) {
    console.error("Error fetching data:", err);
    error.value = err.message;
  } finally {
    loading.value = false;
  }
});
</script>
