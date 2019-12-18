<template>
  <svg id="topo" :width="svg_width" :height="svg_width">

  </svg>
</template>

<script>
  import * as d3 from 'd3';

  export default {
    name: "d3-directed",
    data: function () {
      return {
        svg_width: 700,
        svg_height: 700,
        nodes: [{"asn":1, "value":2}, {"asn":2, "value":2}, {"asn":3, "value":4}],
        links: [{"source":1, "target": 2}, {"source":1, "target": 3}],
      }
    },
    computed: {

    },
    methods: {
      create_links: function () {
        let svg = d3.select("svg#topo");
        console.log(svg);
        svg.append("circle");
        svg.append("g")
          .attr("class", "links")
          .selectAll("line")
          .data(this.links)
          .enter().append("line")
          .attr("stroke-width", d => Math.max(Math.min(4, 0.7* Math.sqrt(0.4*d.value)), 0.6))
          .attr("value", d=>d.value)
          // .attr('marker-end',function(d) {
          //   if (d.shangye == "P2C") return "";
          //   else return 'url(#arrowhead)';
          //   })
          // .attr("marker-start",function(d) {
          //   if (d.shangye == "C2P") return "";
          //   else return 'url(#arrowhead2)';
          //   })
          .attr("id",function (d) {
            return "as" + d.source + "as" + d.target;
          });
      }
    },
    mounted() {
      this.create_links()
    }
  }
</script>

<style scoped>

</style>