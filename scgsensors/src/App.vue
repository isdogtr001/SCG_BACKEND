<template>
  <div>
    <SensorChart />
  </div>
  <div>
    <AggregatedChart />
  </div>
</template>

<script>
import SensorChart from "./components/SensorChart.vue";
import AggregatedChart from "./components/AggregatedChart.vue";

export default {
  name: "App",
  components: {
    SensorChart,
    AggregatedChart,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
