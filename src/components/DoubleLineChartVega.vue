<template>
  <div class="viz doublelinechartvega"></div>
</template>

<script>
import { default as vegaEmbed } from "vega-embed";
import doublelineVegaSpec from "../assets/doublelinevega.json";
import fieldkitHumidityData from "../assets/fieldkitHumidityData.json";
import fieldkitTemperatureData from "../assets/fieldkitTemperatureData.json";

fieldkitHumidityData.data.forEach(item => {
  item.name = "Humidity"
})
fieldkitTemperatureData.data.forEach(item => {
  item.name = "Temperature"
})

doublelineVegaSpec.data[0].values = fieldkitHumidityData.data
doublelineVegaSpec.data[1].values = fieldkitTemperatureData.data

doublelineVegaSpec.axes[1].title = "Humidity (%)";
doublelineVegaSpec.axes[2].title = "Temperature (°F)";

let thresholdsLeft = [
  {
    value: 20,
    label: "Very dry",
    color: "#f1eef6",
  },
  {
    value: 40,
    label: "Dry",
    color: "#bdc9e1",
  },
  {
    value: 60,
    label: "Wet",
    color: "#74a9cf",
  },
  {
    value: 80,
    label: "Very wet",
    color: "#2b8cbe",
  },
  {
    value: 100,
    label: "Very, very wet",
    color: "#045a8d",
  },
];

const thresholdsLeftTransforms = thresholdsLeft.map((d,i) => {
        return {"type": "formula", "expr": "datum.value <= " + d.value  + " ? datum.value : null", "as": "wet" + i}  
})

doublelineVegaSpec.data[0].transform = thresholdsLeftTransforms

let thresholdsRight = [
  {
    value: -40,
    label: "Very cold",
    color: "#fef0d9",
  },
  {
    value: 0,
    label: "Cold",
    color: "#fdcc8a",
  },
  {
    value: 5,
    label: "Regular",
    color: "#fc8d59",
  },
  {
    value: 10,
    label: "Warm",
    color: "#e34a33",
  },
  {
    value: 40,
    label: "Very warm",
    color: "#b30000",
  },
];

const thresholdsRightTransforms = thresholdsRight.map((d,i) => {
        return {"type": "formula", "expr": "datum.value <= " + d.value  + " ? datum.value : null", "as": "warm" + i}  
})

doublelineVegaSpec.data[1].transform = thresholdsRightTransforms

const thresholdsLeftMarks = thresholdsLeft.map((d,i) => {
    return{
        "type": "line",
          "from": { "data": "table1" },
          "encode": {
            "enter": {
              "interpolate": { "value": "cardinal" },
              "tension": { "value": 0.9 },
              "x": { "scale": "x", "field": "time" },
              "y": { "scale": "y", "field": "wet" + i},
              "stroke": { "value": d.color },
              "strokeWidth": { "value": 2 },
              "defined": { "signal": "isValid(datum.wet" + i + ")" }
            },
            "update": {
              "strokeOpacity": {
                "signal": "hover.name == 'Humidity' ? 1 : 0.1"
              }
            }
          }
        }
})
doublelineVegaSpec.marks[0].marks = [doublelineVegaSpec.marks[0].marks[0]].concat(thresholdsLeftMarks.reverse()).concat(doublelineVegaSpec.marks[0].marks[1])

const thresholdsRightMarks = thresholdsRight.map((d,i) => {
    return{
        "type": "line",
          "from": { "data": "table2" },
          "encode": {
            "enter": {
              "interpolate": { "value": "cardinal" },
              "tension": { "value": 0.9 },
              "x": { "scale": "x", "field": "time" },
              "y": { "scale": "y2", "field": "warm" + i},
              "stroke": { "value": d.color },
              "strokeWidth": { "value": 2 },
              "defined": { "signal": "isValid(datum.warm" + i + ")" }
            },
            "update": {
              "strokeOpacity": {
                "signal": "hover.name == 'Temperature' ? 1 : 0.1"
              }
            }
          }
        }
})
doublelineVegaSpec.marks[1].marks = [doublelineVegaSpec.marks[1].marks[0]].concat(thresholdsRightMarks.reverse()).concat(doublelineVegaSpec.marks[1].marks[1])

doublelineVegaSpec.scales = doublelineVegaSpec.scales.concat([
    {
      "name": "colorLeft",
      "type": "ordinal",
      "domain": thresholdsLeft.map(d => d.label),
      "range": thresholdsLeft.map(d => d.color)
    },
    {
      "name": "colorRight",
      "type": "ordinal",
      "domain": thresholdsRight.map(d => d.label),
      "range": thresholdsRight.map(d => d.color)
    }
    ]
)

export default {
  name: "DoubleLineChartVega",
  data() {
    return { vegaView: null };
  },
  mounted: function () {
    vegaEmbed(".doublelinechartvega", doublelineVegaSpec, {
      renderer: "svg",
      tooltip: { offsetX: -50, offsetY: 50 },
      actions: { source: false, editor: false, compiled: false },
    })
  },
};
</script>

<style scoped>
.viz {
  width: 100%;
  height: 100%;
}
</style>
