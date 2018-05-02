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
html {
  background: linear-gradient(-90deg, rgba(227,0,191,1) 0%, rgba(234,26,119,1) 100%);
}

#app {
  color: #FFF;
  font-family: 'Open Sans', Helvetica, Arial, sans-serif;
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
}

h1, h2, h3, h4, h5, h6 {
  font-family: 'Nunito Sans', Helvetica, Arial, sans-serif;
  font-weight: 900;
  margin: 0;
}
</style>
