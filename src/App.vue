<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import HelloWorld from './pages/index.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  }
}
</script>

<style lang="scss">
/* 1. Hilangkan margin/padding dari seluruh browser */
html, body {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
  background-color: #0f1a0f; /* Samakan dengan warna kontenmu */
}

/* 2. Hapus margin-top 60px yang biasanya ada di #app */
#app {
  margin-top: 0 !important; 
  padding: 0;
}
</style>