<template>
  <div id="app">
    <h1>ThreeJS:</h1>
    <Scene3 />
  </div>
</template>

<script>
import Scene3 from './components/Scene3';

export default {
  name: 'app',
  components: {
    Scene3
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
