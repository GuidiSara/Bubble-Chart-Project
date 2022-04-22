<template>
  <h1>Challenge Bubble</h1>
  <div class="cartesianPlane">
    <div id="svgContainer"></div>
    <button @click="changeX()">Cambia x</button>
    <button>Cambia y</button>
  </div>
</template>

<script>
import * as d3 from "d3";
import quantitativeVariablesCircle from "./fileJson/quantitativeVariablesCircle.json";
export default {
  name: "App",
  data: function () {
    return {
      circles: "",
      svg: "",
    };
  },

  mounted() {
    this.createdCircles();
  },
  methods: {
    createdCircles() {
      this.circles = quantitativeVariablesCircle.circles;
      this.svg = d3.select("#svgContainer").append("svg").attr("width", 8000).attr("height", 800);
      this.circles.forEach((circle) => {
        this.svg
          .append("circle")
          .attr("cx", circle.v1)
          .attr("cy", circle.v2)
          .attr("r", circle.v3)
          .attr("stroke", "#5e9f4d")
          .attr("fill", "#b7d2a9");
      });
    },
    changeX() {
      console.log(this.svg);
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
