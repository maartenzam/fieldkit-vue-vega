<template>
  <div class="charts">
    <h1>FieldKit Vega charts</h1>
    <h2>Line chart</h2>
    <div class="chart">
      <DoubleLineChartVega/>
    </div>
    <div class="chart">
      <LineChart v-bind:customColors="false" id="linechart1"></LineChart>
    </div>
    <div class="chart">
      <LineChart v-bind:customColors="true" id="linechart2"></LineChart>
    </div>
    <h2>Line chart with 2 Y-axes</h2>
    <div class="chart">
      <DoubleLineChart />
    </div>
    <div class="chart">
      <DoubleLineChartColor />
    </div>
    <div class="chart">
      <DoubleScrubber />
    </div>
    <h2>Range chart</h2>
    <div class="chart">
      <RangeChart />
    </div>
    <h2>Histogram</h2>
    <div class="chart">
      <Histogram />
    </div>
    <h2>Scrubber</h2>
    <p>
      The selected time range can be read (see console.log) and set (click the
      button).
    </p>
    <div class="chart">
      <Scrubber />
    </div>
  </div>
</template>

<script>
import LineChart from "./components/LineChart.vue";
import DoubleLineChart from "./components/DoubleLineChart.vue";
import DoubleLineChartColor from "./components/DoubleLineChartColor.vue";
import DoubleLineChartVega from "./components/DoubleLineChartVega.vue";
import Histogram from "./components/Histogram.vue";
import RangeChart from "./components/RangeChart.vue";
import Scrubber from "./components/Scrubber.vue";
import DoubleScrubber from "./components/DoubleScrubber.vue";

export default {
  name: "App",
  components: {
    LineChart,
    Histogram,
    RangeChart,
    Scrubber,
    DoubleLineChart,
    DoubleLineChartColor,
    DoubleLineChartVega,
    DoubleScrubber
  },
};
</script>

<style>
@font-face {
  font-family: Avenir Light;
  src: url("/assets/fonts/Avenir/OpenType CFF Pro/AvenirLTPro-Light.otf");
}

.charts {
  width: 80%;
  margin: auto;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
}

@media screen and (max-width: 640px) {
  .charts {
    width: 100%;
  }
}

.chart {
  width: 100%;
  height: 200px;
}
</style>
