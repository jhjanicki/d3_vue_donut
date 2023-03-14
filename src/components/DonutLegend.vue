<template>
  <g v-for="(d, i) in data" :transform="`translate(0,${i * this.categoryHeight})`">
    <text transform="translate(15,9)"> {{d.category}} </text>
    <rect :fill="colorScale(d.category)" width="10" height="10"></rect>
  </g>
</template>

<script>
export default {
  name: "DonutLegend",
  data: () => ({
    categoryHeight: 20,
  }),
  props: ['data','colorScale'],
};
</script>

<style lang="css"></style>
