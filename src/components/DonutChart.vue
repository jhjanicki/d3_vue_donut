<template>
  <svg :width="length + margin * 2" :height="length + margin * 2">
    <g :transform="`translate(${margin + length / 2},${margin + length / 2})`">
      <DonutArc :data="pieData" :colorScale="colorScale" />
      <HiddenArc :data="pieData" />
      <DonutLabel :data="pieData" />
    </g>
    <g :transform="`translate(${length/2-40},${length/2-20})`">
      <DonutLegend :data="dataSummed" :colorScale="colorScale" />
    </g>
  </svg>
</template>

<script>
import * as d3 from "d3";
import data from "../assets/data.json";
import DonutArc from "./DonutArc.vue";
import DonutLabel from "./DonutLabel.vue";
import HiddenArc from "./HiddenArc.vue";
import DonutLegend from "./DonutLegend.vue";

export default {
  name: "DonutChart",
  data: () => ({
    length: 400,
    margin: 30,
  }),
  components: {
    DonutArc,
    DonutLabel,
    HiddenArc,
    DonutLegend,
  },
  computed: {
    data() {
      return data;
    },
    categoriesSorted() {
      return [
        "Critically Endangered",
        "Endangered",
        "Vulnerable",
        "Near Threatened",
        "Least Concern",
        "Data Deficient",
      ];
    },
    colorScale() {
      return d3
        .scaleOrdinal()
        .domain(this.categoriesSorted)
        .range([
          "#b30000",
          "#e34a33",
          "#fc8d59",
          "#fdbb84",
          "#fee8c8",
          "#d9d9d9",
        ]);
    },
    dataSummed() {
      //summarise data & calculate ratio & raw count for each category
      const rolledData = this.data.map((entry) =>
        d3.map(
          d3.rollup(
            this.data,
            (v) => v.length,
            (d) => d.redlistCategory
          ),
          ([category, result]) => ({
            category: category,
            ratio: (result / this.data.length) * 100,
            quantity: result,
          })
        )
      )[0];

      // sort the data based on the order of categories
      const order = {}; // map for efficient lookup of sortIndex
      for (let i = 0; i < this.categoriesSorted.length; i++) {
        order[this.categoriesSorted[i]] = i;
      }

      return rolledData.sort((a, b) => order[a.category] - order[b.category]);
    },
    pieData() {
      //add sort null if want to order segments by category, remove if want to order by ratio
      const pie = d3
        .pie()
        .value((d) => d.ratio)
        .sort(null);
      //pie(data) creates startAngle, endAngle for each data point
      const arcGenerator = d3
        .arc()
        .outerRadius(this.length / 2)
        .innerRadius(this.length / 3);

      return pie(this.dataSummed).map((d) => {
        return {
          data: d.data,
          path: arcGenerator.startAngle(d.startAngle).endAngle(d.endAngle)(),
          fill: this.colorScale(d.data.category),
          centroid: arcGenerator.centroid(d),
        };
      });
    },
  },
};
</script>

<style lang="css"></style>
