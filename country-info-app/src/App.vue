<template>
  <div id="app">
    <CountryDetails/>
  </div>
</template>

<script>
import CountryDetails from './components/CountryDetails.vue';

export default {
  components: {
    CountryDetails
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
