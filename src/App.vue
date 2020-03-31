<template>
  <div id="app">
    <Header></Header>
    <Index></Index>
  </div>
</template>

<script>
import Index from './components/index.vue'
import Header from './components/Header.vue'

export default {
  name: 'App',
  components: {
    Index,
    Header
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
