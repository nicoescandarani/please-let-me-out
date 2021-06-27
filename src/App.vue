<template>
  <div id="app">
    <home v-on:reverse="handleReverse()" />
  </div>
</template>

<script>
import Home from './components/Home.vue';
document.title = 'Please, let me out';

export default {
  name: 'App',
  components: {
    Home
  },
  mounted() {
    const subjectIsIn = localStorage.getItem('subjectIsIn');
    console.log(subjectIsIn);
    if(subjectIsIn) {
      this.handleReverse();
    }
  },
  methods: {
    handleReverse() {
      const app = document.getElementById('app');
      app.classList.add('reverse');
      app.classList.add('new-bg');
    }
  }
}
</script>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  .reverse {
    -moz-transform: scaleX(-1);
    -o-transform: scaleX(-1);
    -webkit-transform: scaleX(-1);
    transform: scaleX(-1);
    filter: FlipH;
    -ms-filter: "FlipH";
  }

  .new-bg {
    /* background-image: url('./assets/gif-test.gif') !important; */
  }

  #app {
    font-family: Arial, Helvetica, sans-serif;
    background-image: url('./assets/bg.jpg');
    background-position: center center;
    background-size: cover;
  }
</style>
