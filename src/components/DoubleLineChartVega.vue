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

const colorLeft = "#5736FF";
const colorRight = "#3fab29";

doublelineVegaSpec.marks[0].marks[1].encode.enter.stroke.value = colorLeft
doublelineVegaSpec.marks[0].marks[2].encode.enter.fill.value = colorLeft
doublelineVegaSpec.marks[1].marks[1].encode.enter.stroke.value = colorRight
doublelineVegaSpec.marks[1].marks[2].encode.enter.fill.value = colorRight

doublelineVegaSpec.axes[1].title = "Humidity (%)";
doublelineVegaSpec.axes[2].title = "Temperature (°F)";

//import chartConfig from "../assets/chartConfig.json";

//doublelineSpec.config = chartConfig;

/*

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
doublelineSpec.layer[1].encoding.y.axis.labelColor = colorRight;*/

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
    }).then((result) => {
      this.vegaView = result;
      this.vegaView.view.addSignalListener("hover", function (_, value) {
        console.log(value)
      });
    });
  },
};
</script>

<style scoped>
.viz {
  width: 100%;
  height: 100%;
}
</style>
