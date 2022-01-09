<template>
  <div id="app">
    <Bingo/>
  </div>
</template>

<script>
import Bingo from './components/Bingo.vue'

export default {
  name: 'App',
  components: {
    Bingo
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
