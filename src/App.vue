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
</style>
