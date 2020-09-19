<template>
  <div id="app">
    <h1>ThreeJS:</h1>
    <Scene2 />
  </div>
</template>

<script>
import Scene2 from './components/Scene2';

export default {
  name: 'app',
  components: {
    Scene2
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
