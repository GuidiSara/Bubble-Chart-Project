<template>
  <h1>Challenge Bubble</h1>
  <div class="cartesianPlane">
    <div id="svgContainer"></div>
  </div>
</template>

<script>
import * as d3 from "d3";
export default {
  name: "App",
  data() {},
  mounted() {
    const width = 150;
    const height = 150;
    let svg1 = d3.select("#svgcontainer").append("svg").attr("width", width).attr("height", height);
    svg1.append("circle").attr("cx", 100).attr("cy", 100).attr("r", 30).attr("fill", "purple");
    let svg2 = d3.select("#svgcontainer").append("svg").attr("width", width).attr("height", height);
    svg2.append("circle").attr("cx", 30).attr("cy", 30).attr("r", 30).attr("fill", "purple");
    let svg3 = d3.select("#svgcontainer").append("svg").attr("width", width).attr("height", height);
    svg3.append("circle").attr("cx", 70).attr("cy", 70).attr("r", 30).attr("fill", "purple");
  },
};
</script>

<style>
h1 {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: red;
  margin-top: 60px;
  margin-bottom: 100px !important;
}
</style>
