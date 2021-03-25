<template>
  <div class="container">
    <div class="text-center">
      <input 
          class="form-control me-2 search" 
          type="search" 
          placeholder="Ex: Sydney" 
          aria-label="Search" 
          v-model="city"
          
          @keyup.enter="fetchWeather"
        >
      <div class="card text-white" v-cloak>
       <div class="custom-control custom-switch text-right">
          <input type="checkbox" class="custom-control-input" id="customSwitch1" @click="toggleUnit">
          <label class="custom-control-label" for="customSwitch1">°F</label>
        </div>
        <div class="text-center my-4" v-if="typeof weather.main != 'undefined'" >
          <h2>{{weather.name}},{{weather.sys.country}}</h2>
          <h1 class="temp mt-4">{{Math.round(weather.main.temp) }}°</h1>
          <h6>Feels like {{Math.round(weather.main.feels_like) }}°</h6>
          <h3 class="my-4 font-italic">{{ weather.weather[0].main }}</h3>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";


export default {
  name: 'Weather',
  data: () => ({
    apiKey: 'd4fae1475eed163c09df0c1e56c77ca3',
    url: 'https://api.openweathermap.org/data/2.5/',
    city: '',
    unit: 'metric',
    weather: {},
  }),
  mounted: function () {
    // On page load
    let long;
    let lat;
    for (var i = 0; i < 2; i++) {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(position => {
          // Fetch weather
          this.locationFound = true;
          long = position.coords.longitude;
          lat = position.coords.latitude;
          fetch(`${this.url}weather/?lat=${lat}&lon=${long}&units=${this.unit}&appid=${this.apiKey}`)
              .then(res => {
                return res.json();
              }).then(res => {
                this.setWeather(res);
              });
          }), (error) => {
            if (error.code == error.PERMISSION_DENIED) {
              this.locationFound = false;
            }
          };
      }
    }
  },
  methods: {
    // for the search bar
      fetchWeather: async function () {
        const fullUrl  = `${this.url}weather/?q=${this.city}&units=${this.unit}&appid=${this.apiKey}`;
            axios
             .get(fullUrl)
             .then(response => (this.weather = response.data))   
      },
      // for location based weather
      setWeather(results) {
      this.weather = results;
      },
      //to toggle between C & F
      toggleUnit: function() {
         if(this.unit == 'metric'){
            this.unit = 'imperial'; 
            alert('Re-enter the city');
         }else{
           this.unit = 'metric';
           alert('Re-enter the city');
         } 
         
      }
  }
  
}
       
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.search{
  max-width:28rem;
  border-radius:30px;
  margin-top:50px;
  box-shadow: 0px 10px 10px;
  display: inline-block;
}
.card{
  max-width:28rem;
  margin:30px auto;
  border-radius:30px;
  background-color: #7f5a83;
  background-image: linear-gradient(315deg, #7f5a83 0%, #0d324d 74%);
}

.temp{
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  font-weight: 700;
}


.custom-switch{
  margin:20px 20px 0px 0px;
}

</style>
