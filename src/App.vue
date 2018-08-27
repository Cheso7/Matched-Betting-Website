<template>
  <div id="app">
    <startingPage/>
  </div>
</template>

<script>
import startingPage from './components/startingPage.vue'

export default {
  name: 'app',
  components: {
    startingPage
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
  margin-top: 0x;
}
</style>
