<template>
  <h1>Weather App</h1>
  <Weather/>
</template>

<script>
import Weather from './components/Weather.vue'

export default {
  name: 'App',
  components: {
    Weather,
    
  }
}
</script>

<style>
#app {
  font-family: Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  padding-top: 30px;
  background-color: #e7eff9;
  background-image: linear-gradient(315deg, #e7eff9 0%, #cfd6e6 74%);
  height:100%;
  padding-bottom: 100px;
}
</style>
