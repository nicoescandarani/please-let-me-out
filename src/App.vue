<template>
  <div id="app">
    <home :title="'Please, let me out'"/>
  </div>
</template>

<script>
import Home from './components/Home.vue';

export default {
  name: 'App',
  components: {
    Home
  }
}
</script>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  #app {
    font-family: Arial, Helvetica, sans-serif;
    background-image: url('./assets/bg.jpg');
    background-position: center center;
    background-size: cover;
  }
</style>
