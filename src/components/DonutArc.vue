<template>
  <path
    v-for="(d, i) in data"
    class="slice"
    :id="`slice_${i}`"
    :d="d.path"
    :fill="colorScale(d.fill)"
    stroke="white"
    stroke-width="1"
  ></path>
</template>

<script>
export default {
  name: "DonutArc",
  props: ['data','colorScale'],
};
</script>

<style lang="css"></style>
