<template>
  <div>
    <div class="header">對焦距離與眼鏡度數計算機</div>
    <GlassesCalculator />
  </div>
</template>

<script>
import GlassesCalculator from './components/GlassesCalculator.vue'

export default {
  name: 'App',
  components: {
    GlassesCalculator
  }
}
</script>

<style>
html {
  font-size: 16px;
}
body {
  margin: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  /* color: #2c3e50; */
  /* margin-top: 60px; */
}
.header {
  background-color: #F0F0F0;
  color: #404040;
  padding: 0.75rem 1rem;
  font-size: 1.25rem;
}
</style>
