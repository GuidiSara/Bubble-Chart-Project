<template>
  <h1>Challenge Bubble</h1>
  <div class="cartesianPlane">
    <svg width="8000" height="800"></svg>
    <button @click="changeX()">Cambia x</button>
    <button @click="changeY()">Cambia y</button>
  </div>
</template>

<script>
import * as d3 from "d3";
import quantitativeVariablesCircle from "./fileJson/quantitativeVariablesCircle.json";

export default {
  name: "App",
  data: function () {
    return {
      svg: "",
      clickX: false,
      clickY: false,
    };
  },

  mounted() {
    this.createdCircles();
  },
  methods: {
    //created circles
    createdCircles() {
      let colorScale = d3
        .scaleOrdinal()
        .domain([1, 23])
        .range(["#D5F5E3", "#ABEBC6", "#82E0AA", "#58D68D", "#2ECC71", "#28B463", "#239B56", "#1D8348", "#186A3B"]);
      let linearScale = d3.scaleLinear().domain([0, 30]).range([0, 800]);
      let rscale = d3.scaleSqrt().range([0, 60]).domain([0, 10]);
      let dataSet = quantitativeVariablesCircle.circles;
      this.svg = d3.select("svg");
      this.svg
        .selectAll("circle")
        .data(dataSet)
        .enter()
        .append("circle")
        .attr("cx", function (d) {
          return linearScale(d.v1);
        })
        .attr("cy", function (d) {
          return linearScale(d.v2);
        })
        .attr("r", function (d) {
          return rscale(d.v3);
        })
        .attr("fill", (d) => colorScale(rscale(d.v3)))
        .attr("stroke", "#186A3B")
        .style("opacity", 0.7);
    },
    changeX() {
      let linearScale = d3.scaleLinear().domain([0, 30]).range([0, 800]);
      let rscale = d3.scaleSqrt().range([0, 60]).domain([0, 10]);
      let colorScale = d3
        .scaleOrdinal()
        .domain([1, 23])
        .range([
          "#EAFAF1",
          "#D5F5E3",
          "#ABEBC6",
          "#82E0AA",
          "#58D68D",
          "#2ECC71",
          "#28B463",
          "#239B56",
          "#1D8348",
          "#186A3B",
        ]);
      if (this.clickX) {
        d3.select("svg")
          .selectAll("circle")
          .attr("cx", function (d) {
            return linearScale(d.v1);
          })
          .transition()
          .duration(1000)
          .attr("r", function (d) {
            return rscale(d.v3);
          })
          .attr("fill", (d) => colorScale(d))
          .attr("stroke", "#186A3B")
          .style("opacity", 0.7);
        this.clickX = false;
      } else {
        d3.select("svg")
          .selectAll("circle")
          .attr("cx", function (d) {
            return linearScale(d.v3);
          })
          .transition()
          .duration(1000)
          .attr("r", function (d) {
            return rscale(d.v1);
          })
          .attr("fill", (d) => colorScale(d))
          .attr("stroke", "#186A3B")
          .style("opacity", 0.7);
        this.clickX = true;
      }
    },
    changeY() {
      let colorScale = d3
        .scaleOrdinal()
        .domain([1, 23])
        .range([
          "#EAFAF1",
          "#D5F5E3",
          "#ABEBC6",
          "#82E0AA",
          "#58D68D",
          "#2ECC71",
          "#28B463",
          "#239B56",
          "#1D8348",
          "#186A3B",
        ]);

      let rscale = d3.scaleSqrt().range([0, 60]).domain([0, 10]);
      let linearScale = d3.scaleLinear().domain([0, 30]).range([0, 800]);
      if (this.clickY) {
        d3.select("svg")
          .selectAll("circle")
          .attr("cy", function (d) {
            return linearScale(d.v2);
          })
          .transition()
          .duration(1000)
          .attr("r", function (d) {
            return rscale(d.v3);
          })
          .attr("fill", (d) => colorScale(d))
          .attr("stroke", "#186A3B")
          .style("opacity", 0.7);
        this.clickY = false;
      } else {
        d3.select("svg")
          .selectAll("circle")
          .attr("cy", function (d) {
            return linearScale(d.v3);
          })
          .transition()
          .duration(1000)
          .attr("r", function (d) {
            return rscale(d.v2);
          })
          .attr("fill", (d) => colorScale(d))
          .attr("stroke", "#186A3B")
          .style("opacity", 0.7);
        this.clickY = true;
      }
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: red;
  margin-top: 60px;
  margin-bottom: 100px !important;
}
</style>
