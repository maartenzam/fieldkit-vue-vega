<template>
  <div class="charts">
    <div class="chart">
      <RangeChart />
    </div>
    <div class="chart">
      <LineChart />
    </div>
    <div class="chart">
      <Histogram />
    </div>
  </div>
</template>

<script>
import LineChart from "./components/LineChart.vue";
import Histogram from "./components/Histogram.vue";
import RangeChart from "./components/RangeChart.vue";

export default {
  name: "App",
  components: {
    LineChart,
    Histogram,
    RangeChart,
  },
};
</script>

<style>
.chart {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  width: 100%;
}
</style>
