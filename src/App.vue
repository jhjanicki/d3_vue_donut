<template>
  <div id="chart">
    <DonutChart />
  </div>
</template>

<script>
import DonutChart from "./components/DonutChart.vue";

export default {
  name: "app",
  components: {
    DonutChart,
  },
};
</script>

<style lang="css">
#chart {
  text-align: center;
  font-family: "Source Sans Pro", sans-serif;
}
</style>
