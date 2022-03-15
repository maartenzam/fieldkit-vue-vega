<template>
  <div>
    <button v-on:click="pickRange([1606624513692, 1607982580082])">
      Pick date range
    </button>
    <div class="viz doublescrubber"></div>
  </div>
</template>

<script>
import { default as vegaEmbed } from "vega-embed";
import scrubberSpec from "../assets/doublescrubber.vl.json";
//import fieldkitBatteryData from "../assets/fieldkitBatteryData.json";
import fieldkitHumidityData from "../assets/fieldkitHumidityData.json";
import fieldkitTemperatureData from "../assets/fieldkitTemperatureData.json";
import iconsData from "../assets/iconsData2.json";
import chartConfig from "../assets/chartConfig.json";

scrubberSpec.config = JSON.parse(JSON.stringify(chartConfig));
const height = 50;

scrubberSpec.height = height;
scrubberSpec.layer[3].encoding.y = { value: height };
scrubberSpec.config.axisX.tickSize = 20;
scrubberSpec.config.view = { fill: "#f4f5f7", stroke: "transparent" };

scrubberSpec.data = { values: fieldkitHumidityData.data };
scrubberSpec.layer[2].data = {values: fieldkitTemperatureData.data};
scrubberSpec.layer[3].data = { values: iconsData.data };

export default {
  name: "Scrubber",
  props: ["showFirst"],
  data() {
    return {
      vegaView: null,
      showfirst: this.showFirst,
      spec: JSON.parse(JSON.stringify(scrubberSpec)),
    };
  },
  computed: {
    finalSpec: function () {
      let finalSpec = this.spec;
      if (this.showfirst) {
        finalSpec.layer[2].mark.opacity = 0;
      } else {
        finalSpec.layer[1].mark.opacity = 0;
      }
      return finalSpec;
    },
  },
  mounted: function () {
    vegaEmbed(".doublescrubber", this.finalSpec, {
      renderer: "svg",
      actions: { source: false, editor: false, compiled: false },
    }).then((result) => {
      this.vegaView = result;
      let timeValue = [];
      this.vegaView.view.addSignalListener("brush", function (_, value) {
        timeValue = value.time;
      });
      this.vegaView.view.addEventListener("mouseup", function () {
        console.log(timeValue);
      });
      let wheeling;
      this.vegaView.view.addEventListener("wheel", function () {
        clearTimeout(wheeling);
        wheeling = setTimeout(function () {
          console.log(timeValue);
        }, 50);
      });
    });
  },
  methods: {
    pickRange: function (timeRange) {
      this.vegaView.view
        .signal("brush_x", [
          this.vegaView.view.scale("x")(timeRange[0]),
          this.vegaView.view.scale("x")(timeRange[1]),
        ])
        .signal("brush_tuple", {
          unit: "layer_0",
          fields: [
            {
              field: "time",
              channel: "x",
              type: "R",
            },
          ],
          values: [timeRange],
        })
        .runAsync();
    },
  },
};
</script>

<style scoped>
.viz {
  width: 100%;
}
</style>
