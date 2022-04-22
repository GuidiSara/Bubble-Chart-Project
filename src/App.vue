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
      let dataSet = quantitativeVariablesCircle.circles;
      this.svg = d3.select("svg");
      this.svg
        .selectAll("circle")
        .data(dataSet)
        .enter()
        .append("circle")
        .attr("cx", function (d) {
          return d.v1;
        })
        .attr("cy", function (d) {
          return d.v2;
        })
        .attr("r", function (d) {
          return d.v3;
        })
        .attr("stroke", "#5e9f4d")
        .attr("fill", "#b7d2a9");
    },
    changeX() {
      console.log(this.clickX);
      if (this.clickX) {
        d3.select("svg")
          .selectAll("circle")
          .attr("cx", function (d) {
            return d.v1;
          })
          .transition()
          .duration(1000)
          .attr("r", function (d) {
            return d.v3;
          })
          .attr("stroke", "#5e9f4d")
          .attr("fill", "#b7d2a9");
        this.clickX = false;
      } else {
        d3.select("svg")
          .selectAll("circle")
          .attr("cx", function (d) {
            return d.v3;
          })
          .transition()
          .duration(1000)
          .attr("r", function (d) {
            return d.v1;
          })
          .attr("stroke", "#5e9f4d")
          .attr("fill", "#b7d2a9");
        this.clickX = true;
      }
    },
    changeY() {
      if (this.clickY) {
        d3.select("svg")
          .selectAll("circle")
          .attr("cy", function (d) {
            return d.v2;
          })
          .transition()
          .duration(1000)
          .attr("r", function (d) {
            return d.v3;
          })
          .attr("stroke", "#5e9f4d")
          .attr("fill", "#b7d2a9");
        this.clickY = false;
      } else {
        d3.select("svg")
          .selectAll("circle")
          .attr("cy", function (d) {
            return d.v3;
          })
          .transition()
          .duration(1000)
          .attr("r", function (d) {
            return d.v2;
          })
          .attr("stroke", "#5e9f4d")
          .attr("fill", "#b7d2a9");
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
