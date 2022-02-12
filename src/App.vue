<template>
  <img alt="Star Wars Logo" src="./assets/swlogo.png">
  <starwarsAPI msg="Welcome to The Star Wars API App"/>
</template>

<script>
import starwarsAPI from './components/starwarsAPI.vue'

export default {
  name: 'App',
  components: {
    starwarsAPI
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: white;
  margin-top: 60px;
  background: black;
  margin: 0;
}
</style>
