<template>
  <h1>Challenge Bubble</h1>
  <div class="cartesianPlane">
    <svg width="8000" height="1000"></svg>
    <button @click="changeX()">Cambia x</button>
    <button @click="changeY()">Cambia y</button>
  </div>
</template>

<script>
import * as d3 from "d3";
import quantitativeVariablesCircle from "./fileJson/quantitativeVariablesCircle.json";

const maxValue = (variable) => {
  let circles = quantitativeVariablesCircle.circles;
  let max;
  if (variable === "x") {
    max = Math.max(
      ...circles.map((c) => {
        return c.v1;
      })
    );
    max = Math.max(
      ...circles.map((c) => {
        return c.v1;
      })
    );
    let objectWithXMax = circles.find((c) => {
      return c.v1 === max;
    });
    max = max + objectWithXMax.v3 / 2;
  } else if (variable === "y") {
    max = Math.max(
      ...circles.map((c) => {
        return c.v2;
      })
    );
    let objectWithYMax = circles.find((c) => {
      return c.v2 === max;
    });
    max = max + objectWithYMax.v3 / 2;
  } else if (variable === "r") {
    max = Math.max(
      ...circles.map((c) => {
        return c.v3;
      })
    );
  }

  return max;
};
const colorScale = d3
  .scaleOrdinal()
  .domain([1, 23])
  .range(["#145A32", "#196F3D", "#1E8449", "#229954", "#27AE60", "#52BE80", "#7DCEA0 ", "#A9DFBF"]);
const linearScaleX = d3
  .scaleLinear()
  .domain([0, maxValue("x")])
  .range([0, 800]);
const linearScaleY = d3
  .scaleLinear()
  .domain([maxValue("y"), 0])
  .range([0, 800]);
const rscale = d3
  .scaleSqrt()
  .range([0, 60])
  .domain([0, maxValue("r")]);
const x_axis = d3.axisBottom(linearScaleX);
const y_axis = d3.axisLeft(linearScaleY);

let correspondingVariables = {
  x: "v1",
  y: "v2",
  r: "v3",
};

export default {
  name: "App",
  data: function () {
    return {
      svg: "",
    };
  },

  mounted() {
    this.createdCircles();
  },
  methods: {
    createdCircles() {
      let dataSet = quantitativeVariablesCircle.circles;
      this.svg = d3.select("svg");
      this.svg
        .selectAll("circle")
        .data(dataSet)
        .enter()
        .append("circle")
        .attr("cx", function (d) {
          return linearScaleX(d.v1);
        })
        .attr("cy", function (d) {
          return linearScaleY(d.v2);
        })
        .attr("r", function (d) {
          return rscale(d.v3);
        })
        .attr("fill", (d) => colorScale(d.v3))
        .attr("stroke", "#186A3B")
        .style("opacity", 0.7);

      this.svg.append("g").attr("transform", "translate(20,800)").call(x_axis);
      this.svg.append("g").attr("transform", "translate(20,0)").call(y_axis);
    },
    changeX() {
      let x = correspondingVariables.x;
      correspondingVariables.x = correspondingVariables.r;
      correspondingVariables.r = x;

      d3.select("svg")
        .selectAll("circle")
        .attr("cx", function (d) {
          return linearScaleX(d[correspondingVariables.x]);
        })
        .transition()
        .duration(1000)
        .attr("r", function (d) {
          return rscale(d[correspondingVariables.r]);
        })
        .attr("fill", (d) => colorScale(d[correspondingVariables.r]))
        .attr("stroke", "#186A3B")
        .style("opacity", 0.7);
    },
    changeY() {
      let y = correspondingVariables.y;
      correspondingVariables.y = correspondingVariables.r;
      correspondingVariables.r = y;
      d3.select("svg")
        .selectAll("circle")
        .attr("cy", function (d) {
          return linearScaleY(d[correspondingVariables.y]);
        })
        .transition()
        .duration(1000)
        .attr("r", function (d) {
          return rscale(d[correspondingVariables.r]);
        })
        .attr("fill", (d) => colorScale(d[correspondingVariables.r]))
        .attr("stroke", "#186A3B")
        .style("opacity", 0.7);
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
