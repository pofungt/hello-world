<template>
  <GetLocation></GetLocation>
  <SearchLocation></SearchLocation>
</template>

<script>
import GetLocation from './components/GetLocation.vue';
import SearchLocation from './components/SearchLocation.vue';

export default {
  name: 'App',
  components: {
    GetLocation,
    SearchLocation
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
