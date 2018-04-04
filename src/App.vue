<template>
  <div id="app">
    <NuevoLibro/>
  </div>
</template>

<script>
import NuevoLibro from './components/nuevo-libro'

export default {
  name: 'App',
  components: {
    NuevoLibro
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
