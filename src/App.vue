<template>
  <h1>Weather App</h1>
  <Weather/>
</template>

<script>
import Weather from './components/Weather.vue'


export default {
  name: 'App',
  components: {
    Weather
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
  background-color: #b3cdd1;
  background-image: linear-gradient(315deg, #b3cdd1 0%, #9fa4c4 74%);

  height:100vh;

}
</style>
