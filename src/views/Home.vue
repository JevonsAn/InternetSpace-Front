<template>
  <div class="home">
    <directed></directed>
  </div>
</template>

<script>
// @ is an alias to /src
import directed from '../components/d3-directed'

export default {
  name: 'home',
  components: {
    directed
  }
}
</script>
