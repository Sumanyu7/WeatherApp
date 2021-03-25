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
  background-color: #b8c6db;
  background-image: linear-gradient(315deg, #b8c6db 0%, #f5f7fa 74%);
  height:100%;
  padding-bottom: 100px;

}
</style>
