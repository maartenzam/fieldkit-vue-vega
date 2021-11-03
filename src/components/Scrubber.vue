<template>
  <div class="viz scrubber"></div>
</template>

<script>
import { default as vegaEmbed } from "vega-embed";
import scrubberSpec from "../assets/scrubber.vl.json";
import fieldkitData from "../assets/fieldkitData.json";

scrubberSpec.data = { values: fieldkitData.data };

export default {
  name: "Scrubber",
  mounted: function () {
    vegaEmbed(".scrubber", scrubberSpec, {
      renderer: "svg",
      //tooltip: { offsetX: -50, offsetY: 50 },
      actions: { source: false, editor: false, compiled: false },
    }).then((result) => {
      result.view.addSignalListener("brush", function (name, value) {
        console.log(value);
      });
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
