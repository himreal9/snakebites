<template>
<h1>🐍Welcome to The Snake Bites🐍</h1>
  <Dice/>
</template>

<script>
import Dice from './components/Dice.vue'

export default {
  name: 'App',
  components: {
    Dice
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
  margin-top: 10px;
  overflow-x: hidden;
}
</style>
