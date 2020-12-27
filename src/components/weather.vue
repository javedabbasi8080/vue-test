<template>
  <div class="home">
      <div class="constainer">
        <center>
        <div class="col-md-8">
        <table class="table">
          <thead>
              <tr>
                <th>city name</th>
                <th>temperature</th>
                <th>maximum temperature</th>
                <th>minimum temperature</th>
                <th>weather icon </th>
              </tr>
          </thead>
          <tbody>
              <tr v-for="item in consolidated_weather" :key="item.id">
                <td>{{title}}</td>
                <td>{{item.the_temp}}</td>
                <td>{{item.max_temp}}</td>
                <td>{{item.min_temp}}</td>
                <td><img :src="setImage(item.weather_state_abbr)" width="20px"></td>
              </tr>
          </tbody>
        </table>
        </div>
      </center>
      </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'weather',
  props: ['woeid'],
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      weatherData: [],
      consolidated_weather:[],
      title:''
    }
  },

  methods: {
    loadWeatherdata(woeid) {
      var self = this;
       axios.get('http://localhost/FrontendTestAssignment/weather.php?command=location&woeid='+woeid)
      .then(res => {
        this.consolidated_weather = res.data.consolidated_weather;
        this.title = res.data.title;
        
      });
   },
   setImage(val){
    var image = 'https://www.metaweather.com/static/img/weather/png/'+val+'.png'
    return image;
   }
 },

   created() {
    
    this.loadWeatherdata(this.woeid);    

  }
}
</script>
