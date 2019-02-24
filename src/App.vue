<template> <!-- About mark up, html... write in this template tag. -->
  <div id="app">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
    <!---------------^^^^^^^^^^^^^^^^^^^^^^^^^^^ pass property msg with message to HelloWorld component --->
    <!-- Open /src/components/HelloWorld.vue to see the way it receives this props -->
  </div>
</template>

<script>
// To use custom component, you need to import first
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'app',
  components: { // Declare the imported component name here to be able to use in <template>....</template>
    HelloWorld
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
