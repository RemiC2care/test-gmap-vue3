<template>
  <GMap :center="center"></GMap>
</template>

<script>
import GMap from './components/GMap.vue'

export default {
  name: 'App',
  components: {
    GMap
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
