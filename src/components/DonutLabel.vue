<template>
  <text v-for="(d, i) in data" class="donutText" dy="-13">
    <textPath startOffset="50%" text-anchor="middle" :href="`#hiddenArc${i}`">
      {{ Math.round(d.data.ratio * 10) / 10 + "%" }}
    </textPath>
  </text>
</template>

<script>
export default {
  name: "DonutLabel",
  data: () => ({}),
  props: ['data'],
};
</script>

<style lang="css"></style>
