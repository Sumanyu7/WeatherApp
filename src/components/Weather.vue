<template>
  <div class="container">
    <div class="weather text-center">
      <input 
          class="form-control me-2 search" 
          type="search" 
          placeholder="Ex: Melbourne,Au" 
          aria-label="Search" 
          v-model="city"
          @keyup.enter="fetchWeather"
      >
      <!-- Current Weather card-->
      <div class="card text-white" v-cloak>
        <div class="custom-control custom-switch text-right">
          <input type="checkbox" class="custom-control-input" id="customSwitch1" @click="toggleUnit">
          <label class="custom-control-label" for="customSwitch1">°F</label>
        </div> 
        <div class="text-center my-4" v-if="typeof one.main != 'undefined'" >
          <h2>{{weather.city.name}}, {{weather.city.country}}</h2>
          <h1 class="temp mt-4">{{Math.round(one.main.temp) }}°</h1>
          <h6>Feels like {{Math.round(one.main.feels_like) }}°</h6>
          <h3 class="my-4 font-italic">{{ one.weather[0].description }} </h3>
        </div>
      </div>
    </div>
    <!-- 4 day forecast cards-->
    <div class="row row-cols-1 row-cols-md-4 g-4 text-center" >
      <div class="col">
        <div class="card text-white" v-cloak>
          <div class="text-center my-4" v-if="typeof two.main != 'undefined'" >
            <p>{{two.dt_txt}}</p>
            <h1 class="temp mt-4">{{Math.round(two.main.temp) }}°</h1>
            <h6> {{two.weather[0].description  }}</h6>
          </div>
        </div>
      </div>
      <div class="col">
        <div class="card text-white" v-cloak>
          <div class="text-center my-4" v-if="typeof three.main != 'undefined'" >
            <p>{{three.dt_txt}}</p>
            <h1 class="temp mt-4">{{Math.round(three.main.temp) }}°</h1>
            <h6> {{three.weather[0].description  }}</h6>
          </div>
        </div>
      </div>
      <div class="col">
        <div class="card text-white" v-cloak>
          <div class="text-center my-4" v-if="typeof four.main != 'undefined'" >
            <p>{{four.dt_txt}}</p>
            <h1 class="temp mt-4">{{Math.round(four.main.temp) }}°</h1>
            <h6> {{four.weather[0].description }}</h6>
          </div>
        </div>
      </div>
      <div class="col">
        <div class="card text-white" v-cloak>
          <div class="text-center my-4" v-if="typeof five.main != 'undefined'" >
            <p>{{five.dt_txt}}</p>
            <h1 class="temp mt-4">{{Math.round(five.main.temp) }}°</h1>
            <h6>{{five.weather[0].description }}</h6>  
          </div>
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
    url: 'https://api.openweathermap.org/data/2.5/forecast',
    city: '',
    unit: 'metric',
    weather: {},
    //sebsequent days of the week if today is one
    one: {},
    two: {},
    three: {},
    four: {},
    five: {},
  }),
  mounted: function () {
    // for location based weather happens on page load
    let long;
    let lat;
    for (var i = 0; i < 2; i++) {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(position => {
          this.locationFound = true;
          long = position.coords.longitude;
          lat = position.coords.latitude;
          fetch(`${this.url}?lat=${lat}&lon=${long}&units=${this.unit}&appid=${this.apiKey}`)
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
        const fullUrl  = `${this.url}?q=${this.city}&units=${this.unit}&appid=${this.apiKey}`;
            axios
             .get(fullUrl)
             .then(response => (this.setWeather(response.data)))   
      },
      //to set the weather to different days 
      setWeather(results) {
      this.weather = results;
      this.one = this.weather.list[0];
      this.two = this.weather.list[7];
      this.three = this.weather.list[15];
      this.four = this.weather.list[23];
      this.five = this.weather.list[31];
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

<!-- Added "scoped" attribute to limit CSS to this component only -->
<style scoped>
.search{
  max-width:28rem;
  border-radius:30px;
  margin-top:50px;
  box-shadow: 0px 10px 10px;
  display: inline-block;
}
.weather .card{
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
.col .card{
  background-color: #b1bfd8;
  background-image: linear-gradient(315deg, #8ca7d6 0%, #244480 74%);
  border-radius:30px;
}
</style>
