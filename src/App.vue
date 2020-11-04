<template>
  <div id="app">
    <main-part/>
  </div>
</template>

<script>
import MainSection from './views/main'


export default {
  name: 'App',
  components: {
    'main-part': MainSection,
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Segoe UI', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #000000;
  margin-top: 60px;
}
</style>
