<template>
  <h1>Challenge Bubble</h1>
  <div class="cartesianPlane">
    <svg id="svg"></svg>
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
  .domain([1, maxValue("r")])
  .range(["#3D5C5C", "#366363", "#2E6B6B", "#267373", "#1F7A7A", "#178282", "#0F8A8A", "#089191 ", "#009999"]);
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

const changeX = () => {
  let r = correspondingVariables.r;
  let x = correspondingVariables.x;
  correspondingVariables.x = correspondingVariables.r;
  correspondingVariables.r = x;

  d3.select("svg")
    .selectAll("circle")
    .transition()
    .duration(2000)
    .attr("cx", function (d) {
      return linearScaleX(d[correspondingVariables.x]);
    })
    .attr("transform", "translate(" + r + ", 0)")

    .attr("r", function (d) {
      return rscale(d[correspondingVariables.r]);
    })
    .attr("fill", (d) => colorScale(d[correspondingVariables.r]))
    .attr("stroke", "#A8C2A3")
    .style("opacity", 0.7);
};
const changeY = () => {
  let y = correspondingVariables.y;
  correspondingVariables.y = correspondingVariables.r;
  correspondingVariables.r = y;
  d3.select("svg")
    .selectAll("circle")
    .transition()
    .duration(2000)
    .attr("cy", function (d) {
      return linearScaleY(d[correspondingVariables.y]);
    })
    .attr("r", function (d) {
      return rscale(d[correspondingVariables.r]);
    })
    .attr("fill", (d) => colorScale(d[correspondingVariables.r]))
    .attr("stroke", "#A8C2A3")
    .style("opacity", 0.7);
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
        .attr("stroke", "#A8C2A3")
        .style("opacity", 0.7);

      this.createdAxis();
    },

    createdAxis() {
      this.svg
        .append("g")
        .attr("class", "x-axis")
        .attr("transform", "translate(20,800)")
        .on("click", function () {
          changeX();
        })
        .call(x_axis);
      this.svg
        .append("g")
        .attr("class", "y-axis")
        .attr("transform", "translate(20,0)")
        .on("click", function () {
          changeY();
        })
        .call(y_axis);
    },
  },
};
</script>

<style>
#app {
  font-family: Arial, Helvetica, sans-serif;
  text-align: center;
  margin-top: 60px;
}
h1 {
  color: #1686fe;
  font-size: 2.5em;
  opacity: 0.7;
}

.cartesianPlane {
  display: flex;
  justify-content: center;
}
svg {
  width: 800px;
  height: 1000px;
}
.x-axis line,
.x-axis path,
.x-axis text,
.y-axis line,
.y-axis path,
.y-axis text {
  stroke: black;
  font-family: Arial, Helvetica, sans-serif;
  cursor: pointer;
}
</style>
