<template>
  <div class="viz rangechart"></div>
</template>

<script>
import { default as vegaEmbed } from "vega-embed";
import rangeSpec from "../assets/range.vl.json";
import fieldkitData from "../assets/fieldkitData.json";
import chartConfig from "../assets/chartConfig.json";

rangeSpec.config = chartConfig;
rangeSpec.data = { values: fieldkitData.data, name: "rangedata" };

export default {
  name: "RangeChart",
  mounted: function () {
    vegaEmbed(".rangechart", rangeSpec, {
      renderer: "svg",
      tooltip: { offsetX: -50, offsetY: 50 },
      actions: { source: false, editor: false, compiled: false },
    }); /*.then(function (result) {
        console.log(result.view._runtime);
      })
      .catch(console.error);*/
  },
};
</script>

<style scoped>
.viz {
  width: 100%;
}
</style>
