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
//import fieldkitBatteryData from "../assets/fieldkitBatteryData.json";
import fieldkitBatteryData from "../assets/fieldkitPhMissingData.json";
import chartConfig from "../assets/chartConfig.json";

lineSpec.config = chartConfig;
//lineSpec.config.axis.grid = true;

lineSpec.data = { values: fieldkitBatteryData.data };

export default {
  name: "LineChart",
  mounted: function () {
    vegaEmbed(".linechart", lineSpec, {
      renderer: "svg",
      tooltip: { offsetX: -50, offsetY: 50 },
      actions: { source: false, editor: false, compiled: false },
    }).then((result) => {
      this.vegaView = result;
      result.view.addSignalListener("brush", function (_, value) {
        console.log(value.time);
      });
    });
  },
  methods: {
    // From https://vega.github.io/vega/docs/api/view/#view_toImageURL
    downloadChart: function (fileFormat) {
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
