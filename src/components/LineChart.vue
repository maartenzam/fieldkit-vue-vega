<template>
  <div>
    <button v-on:click="downloadChart('png')">Download chart png</button>
    <button v-on:click="downloadChart('svg')">Download chart svg</button>
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
    }).then((result) => {
      this.vegaView = result;
    });
  },
  methods: {
    downloadChart: function (fileFormat) {
      console.log(fileFormat);
      this.vegaView.view
        .toImageURL(fileFormat, 2)
        .then(function (url) {
          var link = document.createElement("a");
          link.setAttribute("href", url);
          link.setAttribute("target", "_blank");
          link.setAttribute("download", "vega-export." + fileFormat);
          link.dispatchEvent(new MouseEvent("click"));
        })
        .catch(function (error) {
          console.log(error);
        });
    },
  },
};
</script>

<style scoped>
.viz {
  width: 100%;
}
</style>
