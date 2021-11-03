<template>
  <div class="viz rangechart"></div>
</template>

<script>
import { default as vegaEmbed } from "vega-embed";
import rangeSpec from "../assets/range.vl.json";
import fieldkitData from "../assets/fieldkitData.json";

rangeSpec.data = { values: fieldkitData.data, name: "rangedata" };

export default {
  name: "RangeChart",
  mounted: function () {
    vegaEmbed(".rangechart", rangeSpec, {
      renderer: "svg",
      tooltip: { offsetX: -50, offsetY: 50 },
      actions: { source: false, editor: false, compiled: false },
    });
    /*.then(function (result) {
        console.log(result.view.data("rangedata"));
      })
      .catch(console.error);*/
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
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
