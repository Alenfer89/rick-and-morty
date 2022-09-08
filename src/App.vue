<template>
  <div id="app">

    <Main />
    
  </div>
</template>

<script>
//components
import Main from './pages/Main.vue';

export default {
  name: 'App',
  components: {
    Main,
  }
}
</script>

<style lang="scss">
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
  }
</style>
