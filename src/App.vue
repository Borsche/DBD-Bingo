<template>
  <div id="app">
    <bingo/>
    <a href="https://github.com/Borsche/DBD-Bingo">Github</a>
  </div>
</template>

<script>
//import Bingo from './components/Bingo.vue'
import Bingo from '@/components/Bingo.vue'

export default {
  name: 'App',
  components: {
    Bingo,
    //BingoField
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #dce0e4;
  margin-top: 60px;
}

body {
  background-image: url('assets/background.jpg');
  background-repeat: no-repeat;
  background-color: #333131;
  background-position: center;
}
</style>
