<template>
  <h1>Challenge Bubble</h1>
  <div class="cartesianPlane">
    <svg id="svg">
      <defs>
        <radialGradient id="gradient">
          <stop offset="0%" stop-color="rgba(32,171,177,1)" />
          <stop offset="59%" stop-color="rgba(23,152,157,1" />
          <stop offset="88%" stop-color="rgba(16,131,136,1)" />
        </radialGradient>
      </defs>
    </svg>
  </div>
</template>

<script>
import * as d3 from "d3";
import quantitativeVariablesCircle from "./fileJson/quantitativeVariablesCircle.json";
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
    this.createCircles();
  },
  methods: {
    maxValue(variable) {
      let circles = quantitativeVariablesCircle.circles;
      let max;
      let x = correspondingVariables.x;
      let y = correspondingVariables.y;
      let r = correspondingVariables.r;

      if (variable === "x") {
        max = Math.max(
          ...circles.map((c) => {
            return c[x];
          })
        );
        let objectWithXMax = circles.find((c) => {
          return c[x] === max;
        });
        max = max + objectWithXMax[r];
      } else if (variable === "y") {
        max = Math.max(
          ...circles.map((c) => {
            return c[y];
          })
        );
        let objectWithYMax = circles.find((c) => {
          return c[y] === max;
        });

        max = max + objectWithYMax[r];
      } else if (variable === "r") {
        max = Math.max(
          ...circles.map((c) => {
            return c[r];
          })
        );
      }
      return max;
    },
    getScales() {
      const linearScaleX = d3
        .scaleLinear()
        .domain([0, this.maxValue("x")])
        .range([0, 800]);
      const linearScaleY = d3
        .scaleLinear()
        .domain([this.maxValue("y"), 0])
        .range([0, 800]);
      const rscale = d3
        .scaleSqrt()
        .range([0, 60])
        .domain([0, this.maxValue("r")]);
      return { scaleX: linearScaleX, scaleY: linearScaleY, scaleR: rscale };
    },
    createCircles() {
      let dataSet = quantitativeVariablesCircle.circles;
      const { scaleX, scaleY, scaleR } = this.getScales();
      this.svg = d3.select("svg");
      this.svg
        .selectAll("circle")
        .data(dataSet)
        .enter()
        .append("circle")
        .attr("cx", function (d) {
          return scaleX(d.v1);
        })
        .attr("cy", function (d) {
          return scaleY(d.v2);
        })
        .attr("r", function (d) {
          return scaleR(d.v3);
        })
        .attr("stroke", "#0c7378")
        .attr("fill", "url(#gradient)")
        .style("opacity", 0.7);

      this.createAxis();
    },
    changeX() {
      let x = correspondingVariables.x;
      const { scaleX, scaleR } = this.getScales();
      correspondingVariables.x = correspondingVariables.r;
      correspondingVariables.r = x;
      d3.select("svg")
        .selectAll("circle")
        .transition()
        .duration(2000)
        .attr("cx", function (d) {
          return scaleX(d[correspondingVariables.x]);
        })
        .attr("r", function (d) {
          return scaleR(d[correspondingVariables.r]);
        });

      this.deleteAxis();
      this.createAxis();
    },

    changeY() {
      let y = correspondingVariables.y;
      correspondingVariables.y = correspondingVariables.r;
      correspondingVariables.r = y;
      const { scaleY, scaleR } = this.getScales();
      d3.select("svg")
        .selectAll("circle")
        .transition()
        .duration(2000)
        .attr("cy", function (d) {
          return scaleY(d[correspondingVariables.y]);
        })
        .attr("r", function (d) {
          return scaleR(d[correspondingVariables.r]);
        });

      this.deleteAxis();
      this.createAxis();
    },

    createAxis() {
      const { scaleX, scaleY } = this.getScales();
      const x_axis = d3.axisBottom(scaleX);
      const y_axis = d3.axisLeft(scaleY);

      this.svg
        .append("g")
        .attr("class", "x-axis")
        .attr("transform", "translate(20,800)")
        .on("click", () => {
          this.changeX();
        })
        .call(x_axis);
      this.svg
        .append("g")
        .attr("class", "y-axis")
        .attr("transform", "translate(20,0)")
        .on("click", () => {
          this.changeY();
        })
        .call(y_axis);
    },

    deleteAxis() {
      this.svg.selectAll("g").remove();
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
  color: #089191;
  font-size: 2.5em;
  opacity: 0.7;
}

.cartesianPlane {
  display: flex;
  justify-content: center;
}
svg {
  width: 800px;
  height: 830px;
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
