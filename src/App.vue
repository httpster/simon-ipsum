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

<style lang="scss">
@import './scss/app.scss';
</style>
