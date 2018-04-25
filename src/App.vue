<template>
  <div id="app">
    <SimonIpsum/>
  </div>
</template>

<script>
import SimonIpsum from './components/SimonIpsum'

export default {
  name: 'App',
  components: {
    SimonIpsum
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
