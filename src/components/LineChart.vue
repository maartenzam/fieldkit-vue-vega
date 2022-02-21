<template>
  <div>
    <button v-on:click="downloadChart('png')">Download chart png</button>
    <button v-on:click="downloadChart('svg')">Download chart svg</button>
    <div class="viz linechart" :id="this.id"></div>
  </div>
</template>

<script>
import { default as vegaEmbed } from "vega-embed";
import lineSpec from "../assets/line.vl.json";
//import fieldkitBatteryData from "../assets/fieldkitBatteryData.json";
import fieldkitBatteryData from "../assets/fieldkitPhMissingData.json";
import chartConfig from "../assets/chartConfig.json";

lineSpec.config = chartConfig;
lineSpec.data = { values: fieldkitBatteryData.data };

let thresholds = [
  {
    value: 8,
    label: "No Flooding",
    color: "#00CCFF",
  },
  {
    value: 8.5,
    label: "Almost no flooding",
    color: "#0099FF",
  },
  {
    value: 9,
    label: "A little bit of flooding",
    color: "#0066FF",
  },
  {
    value: 9.5,
    label: "Flooding",
    color: "#0033FF",
  },
  {
    value: 10,
    label: "A lot of flooding",
    color: "#0000FF",
  },
];

const thresholdLayers = thresholds
  .map((d, i) => {
    return {
      transform: [
        {
          calculate: "datum.value <= " + d.value + " ? datum.value : null",
          as: "layerValue" + i,
        },
        {
          calculate:
            "datum.layerValue" +
            i +
            " <= " +
            d.value +
            " ? '" +
            d.label +
            "' : null",
          as: "Grade of flooding",
        },
      ],
      encoding: {
        y: { field: "layerValue" + i },
        stroke: {
          field: "Grade of flooding",
          legend: {
            orient: "top",
          },
          scale: {
            domain: thresholds.map((d) => d.label),
            range: thresholds.map((d) => d.color),
          },
        },
      },
      mark: {
        type: "line",
        interpolate: { expr: "interpolate" },
        tension: { expr: "tension" },
      },
    };
  })
  .reverse();

export default {
  name: "LineChart",
  props: ["customColors", "id"],
  data: function () {
    return {
      customcolors: this.customColors,
      spec: JSON.parse(JSON.stringify(lineSpec)),
    };
  },
  computed: {
    finalSpec: function () {
      let finalSpec = this.spec;
      if (this.customcolors) {
        //Overwrite line layer
        finalSpec.layer[0].layer[1].layer = thresholdLayers;
        //Remove layer with dots
        finalSpec.layer[0].layer.splice(2,1)
      }
      return finalSpec;
    },
  },
  mounted: function () {
    vegaEmbed("#" + this.id, this.finalSpec, {
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
