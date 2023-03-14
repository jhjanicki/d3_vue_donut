<template>
  <path
    v-for="(d, i) in data"
    class="hiddenArcs"
    :id="`hiddenArc${i}`"
    :d="calculatePath(d.path)"
    fill="none"
  ></path>
</template>

<script>
export default {
  name: "HiddenArc",
  data: () => ({}),
  props: ['data'],
  methods: {
    calculatePath(d) {
        const firstArcSection = /(^.+?)L/;
        let hiddenArc = firstArcSection.exec(d)[1];
        return  hiddenArc.replace(/,/g, " ");
    },
  },
};
</script>

<style lang="css"></style>
