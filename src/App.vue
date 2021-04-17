<template>
  <div id="app">
    <Container msg="Welcome to Conformit VueJS Test By Maàmoun Grissa"/>
  </div>
</template>

<script>
import Container from './components/Container.vue'

export default {
  name: 'App',
  components: {
    Container
  }
}
</script>

<style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }
  /* width */
  ::-webkit-scrollbar {
    width: 3px;
  }

  /* Track */
  ::-webkit-scrollbar-track {
    background: #f1f1f1;
  }

  /* Handle */
  ::-webkit-scrollbar-thumb {
    background: #93C5FD;
    border-radius: 20px;
    opacity: 0.8;
  }

  /* Handle on hover */
  ::-webkit-scrollbar-thumb:hover {
    background: #EFF6FF;
  }
</style>
