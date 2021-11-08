<template>
  <div>
    <button v-on:click="pickRange">Pick date range</button>
    <div class="viz scrubber"></div>
  </div>
</template>

<script>
import { default as vegaEmbed } from "vega-embed";
import scrubberSpec from "../assets/scrubber.vl.json";
import fieldkitData from "../assets/fieldkitData.json";
import iconsData from "../assets/iconsData.json";
import chartConfig from "../assets/chartConfig.json";

scrubberSpec.config = chartConfig;

scrubberSpec.data = { values: fieldkitData.data };
scrubberSpec.layer[2].data = { values: iconsData.data };

export default {
  name: "Scrubber",
  data() {
    return { dateRange: { time: [1630044000000, 1631577600000] } };
  },
  mounted: function () {
    vegaEmbed(".scrubber", scrubberSpec, {
      renderer: "svg",
      actions: { source: false, editor: false, compiled: false },
    }).then((result) => {
      result.view.addSignalListener("brush", function (name, value) {
        console.log(value);
      });
    });
  },
  methods: {
    pickRange: function () {
      vegaEmbed(".scrubber", scrubberSpec, {
        renderer: "svg",
        actions: { source: false, editor: false, compiled: false },
      }).then((result) => {
        result.view.addSignalListener("brush", function (_, value) {
          console.log(value);
        });
        result.view.signal("brush_tuple", {
          unit: "layer_0",
          fields: [
            {
              field: "time",
              channel: "x",
              type: "R",
            },
          ],
          values: [[1629956332434, 1630344275971]],
        });
        result.view.runAsync();
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
