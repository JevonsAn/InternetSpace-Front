<template>
  <div class="home">
    <directed :links="links" :nodes="nodes"></directed>
  </div>
</template>

<script>
// @ is an alias to /src
import directed from '../components/d3-directed'
import data_json from "../assets/miserables"
export default {
  name: 'home',
  data: function () {
    return {
      links: data_json.links,
      nodes: data_json.nodes,
    }
  },
  components: {
    directed
  }
}
</script>
