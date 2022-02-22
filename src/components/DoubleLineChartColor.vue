<template>
  <div class="viz doublelinechartcolor"></div>
</template>

<script>
import { default as vegaEmbed } from "vega-embed";
import doublelineSpec from "../assets/doublelinecolor.vl.json";
import fieldkitHumidityData from "../assets/fieldkitHumidityData.json";
import fieldkitTemperatureData from "../assets/fieldkitTemperatureData.json";
import chartConfig from "../assets/chartConfig.json";

doublelineSpec.config = chartConfig;

const colorLeft = "#5736FF";
const colorRight = "#3fab29";

fieldkitHumidityData.data.forEach(item => {
  item.name = "Humidity"
})
fieldkitTemperatureData.data.forEach(item => {
  item.name = "Temperature"
})


doublelineSpec.layer[0].data = { values: fieldkitHumidityData.data };
doublelineSpec.layer[0].encoding.y.title = "Humidity (%)";
doublelineSpec.layer[0].layer[1].mark.color = colorLeft;
doublelineSpec.layer[0].layer[2].mark.color = colorLeft;
doublelineSpec.layer[0].encoding.y.axis.titleColor = colorLeft;
doublelineSpec.layer[0].encoding.y.axis.gridColor = colorLeft;
doublelineSpec.layer[0].encoding.y.axis.labelColor = colorLeft;

doublelineSpec.layer[1].data = { values: fieldkitTemperatureData.data };
doublelineSpec.layer[1].encoding.y.title = "Temperature (°F)";
doublelineSpec.layer[1].layer[1].mark.color = colorRight;
doublelineSpec.layer[1].layer[2].mark.color = colorRight;
doublelineSpec.layer[1].encoding.y.axis.titleColor = colorRight;
doublelineSpec.layer[1].encoding.y.axis.gridColor = colorRight;
doublelineSpec.layer[1].encoding.y.axis.labelColor = colorRight;

export default {
  name: "DoubleLineChart",
  mounted: function () {
    vegaEmbed(".doublelinechartcolor", doublelineSpec, {
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
}
</style>
