# fieldkit-vue-vega

Vega-Lite spec and Vega-embed integration in Vue for FieldKit sensor data.

## Structure

The Vega-Lite specifications for the charts are in [src/assets](src/assets), and are named CHARTTYPE.vl.json. Each chart has a matching component in [src/components](src/components).

The data for this demo consists of 2 timeseries (fieldKitBatteryData.json and fieldKitWindSpeedData.json) which were taken directly from the FieldKit platform. These alse live in src/assets, as is iconsData.json, a little data file to add icons to the scrubber component.

Each chart component

- imports its Vega-Lite spec
- imports the needed data
- imports the shared chart styling from [src/assets/chartConfig.json](src/assets/chartConfig.json)
- merges the shared styling and the data into the spec
- uses vega-embed to embed the chart in the component

## Functionality

All charts but the scrubber have tooltips. The content of the tooltips for some of the charts is still to be determined (those were not included in the Zeplin designs).

With Vega-embed it is easy to include links to download png and svg versions of a chart (see the circle with the 3 dots in the top right corner of each chart). But this functionality can also be offered outside of Vega-embed. This is implemented with two download buttons in the LineChart component.

The scrubber component's selected time range can be read (it is now logged when the selected range changes), and it can be set from outside (see the "Pick date range" button in the component).

Both time series component expose a dropdown to pick an interpolation method. For some of these, a tension parameter can adjust the interpolation method.

The charts are responsive, but unfortunately Vega-Lite [does not support touch events yet](https://github.com/vega/roadmap/issues/6), so the scrubber does not work on mobile devices.
