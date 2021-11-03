<template>
  <div class="charts">
    <div class="chart">
      <DoubleLineChart />
    </div>
    <div class="chart">
      <Scrubber />
    </div>
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
import Scrubber from "./components/Scrubber.vue";
import LineChart from "./components/LineChart.vue";
import Histogram from "./components/Histogram.vue";
import RangeChart from "./components/RangeChart.vue";
import DoubleLineChart from "./components/DoubleLineChart.vue";

export default {
  name: "App",
  components: {
    LineChart,
    Histogram,
    RangeChart,
    Scrubber,
    DoubleLineChart,
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
