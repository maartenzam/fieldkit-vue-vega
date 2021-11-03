<template>
  <div class="viz histogram"></div>
</template>

<script>
import { default as vegaEmbed } from "vega-embed";
import histoSpec from "../assets/histo.vl.json";
import fieldkitData from "../assets/fieldkitData.json";

histoSpec.data = { values: fieldkitData.data };

export default {
  name: "LineChart",
  mounted: function () {
    vegaEmbed(".histogram", histoSpec, {
      renderer: "svg",
      tooltip: { offsetX: -50, offsetY: 50 },
      actions: { source: false, editor: false, compiled: false },
    });
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
