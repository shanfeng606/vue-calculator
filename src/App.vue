<template>
  <div id="app">
      <Calculate></Calculate>
  </div>
</template>

<script>
import Calculate from './components/calculate'
export default {
  name: 'App',
  components: {
    Calculate
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  /* margin-top: 60px; */
}
body{
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    background-color: #eee;
}
</style>
