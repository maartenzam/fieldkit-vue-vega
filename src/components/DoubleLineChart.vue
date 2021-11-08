<template>
  <div class="viz doublelinechart"></div>
</template>

<script>
import { default as vegaEmbed } from "vega-embed";
import doublelineSpec from "../assets/doubleline.vl.json";
import fieldkitData from "../assets/fieldkitData.json";
import fieldkitWindSpeedData from "../assets/fieldkitWindSpeedData.json";
import chartConfig from "../assets/chartConfig.json";

doublelineSpec.config = chartConfig;

doublelineSpec.layer[0].data = { values: fieldkitData.data };
doublelineSpec.layer[1].data = { values: fieldkitWindSpeedData.data };

export default {
  name: "DoubleLineChart",
  mounted: function () {
    vegaEmbed(".doublelinechart", doublelineSpec, {
      renderer: "svg",
      tooltip: { offsetX: -50, offsetY: 50 },
      actions: { source: false, editor: false, compiled: false },
    });
  },
};
</script>

<style scoped>
@font-face {
  font-family: Avenir Light;
  src: url("../assets/fonts/Avenir/OpenType CFF Pro/AvenirLTPro-Light.otf");
}
.viz {
  width: 100%;
}
.mark-text.role-axis-label {
  font-family: "Avenir Light", sans-serif;
}
</style>
