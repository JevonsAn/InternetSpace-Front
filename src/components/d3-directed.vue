<template>
  <svg id="topo" :width="svg_width" :height="svg_width">

  </svg>
</template>

<script>
  import * as d3 from 'd3';


  export default {
    name: "d3-directed",
    props: {
      links: Array,
      nodes: Array,
    },
    data: function () {
      return {
        svg_width: 700,
        svg_height: 700,
      }
    },
    computed: {
      data_merge: function () {
        return {
          "links": this.links,
          "nodes": this.nodes
        }
      }
    },
    methods: {
      draw: function () {
        const simulation = d3.forceSimulation(this.nodes)
          .force("link", d3.forceLink(this.links).id(d => d.id))
          .force("charge", d3.forceManyBody())
          .force("center", d3.forceCenter(this.svg_width / 2, this.svg_height / 2));

        const svg = d3.select("svg#topo");

        const link = svg.append("g")
          .attr("stroke", "#999")
          .attr("stroke-opacity", 0.6)
          .selectAll("line")
          .data(this.links)
          .join("line")
          .attr("stroke-width", d => Math.sqrt(d.value));

        const scale = d3.scaleOrdinal(d3.schemeCategory10);
        const node = svg.append("g")
          .attr("stroke", "#fff")
          .attr("stroke-width", 1.5)
          .selectAll("circle")
          .data(this.nodes)
          .join("circle")
          .attr("r", 5)
          .attr("fill", d => scale(d.group))
          .call(drag(simulation));

        node.append("title")
          .text(d => d.id);

        simulation.on("tick", () => {
          link
            .attr("x1", d => d.source.x)
            .attr("y1", d => d.source.y)
            .attr("x2", d => d.target.x)
            .attr("y2", d => d.target.y);

          node
            .attr("cx", d => d.x)
            .attr("cy", d => d.y);
        });

        // invalidation.then(() => simulation.stop());
        // return svg.node();
      },

    },
    watch: {
      data_merge: function () {
        this.draw();
      },
    },
    mounted() {
      this.draw();
    }

  }

  let drag = simulation => {

    function dragstarted(d) {
      if (!d3.event.active) simulation.alphaTarget(0.3).restart();
      d.fx = d.x;
      d.fy = d.y;
    }

    function dragged(d) {
      d.fx = d3.event.x;
      d.fy = d3.event.y;
    }

    function dragended(d) {
      if (!d3.event.active) simulation.alphaTarget(0);
      d.fx = null;
      d.fy = null;
    }

    return d3.drag()
      .on("start", dragstarted)
      .on("drag", dragged)
      .on("end", dragended);
  };

</script>

<style scoped>

</style>

<style>
  .link line {
    stroke: #333;
    stroke-opacity: 0.6;
  }
  /*.node circle {
    stroke: #333;
    stroke-width: 1px;
    stroke-opacity: 0.6;
  }*/
  .node{
    stroke: #333;
    stroke-width: 1px;
    stroke-opacity: 0.6;
  }
</style>