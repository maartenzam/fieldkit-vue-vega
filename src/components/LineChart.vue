<template>
  <div>
    <button v-on:click="downloadChart">Download chart</button>
    <div class="viz linechart"></div>
  </div>
</template>

<script>
import { default as vegaEmbed } from "vega-embed";
import lineSpec from "../assets/line.vl.json";
import fieldkitData from "../assets/fieldkitData.json";
import chartConfig from "../assets/chartConfig.json";

lineSpec.config = chartConfig;

lineSpec.data = { values: fieldkitData.data };

export default {
  name: "LineChart",
  mounted: function () {
    vegaEmbed(".linechart", lineSpec, {
      renderer: "svg",
      tooltip: { offsetX: -50, offsetY: 50 },
      actions: { source: false, editor: false, compiled: false },
    });
  },
};
</script>

<style scoped>
.viz {
  width: 100%;
}
</style>
