<template>
  <div class="home">
      <div class="col-md-12">
        <label>Search</label>
        <input type="text" name="search" id="search_in" class="form-contoller">
        <button type="button" name="send" @click="search()">Search</button>
      </div>
      <div class="tes" v-for="item in woeidData" :key="item.id">
    	<weather :woeid="item.id"></weather>
      </div>
  </div>
</template>

<script>
import weather from './Weather'
import axios from 'axios'

export default {
  name: 'Home',
  components: {
    'weather': weather,
  },
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      locations:['Istanbul', 'Berlin', 'London', 'Helsinki', 'Dublin', 'Vancouver'],
      woeidData:[]
    }
  },


  methods: {
    loadWeather(location) {
      console.log(location);
      var self = this;
       axios.get('http://localhost/FrontendTestAssignment/weather.php?command=search&keyword='+location)
      .then(res => {

        var id = res.data[0].woeid;
        this.woeidData.push({id:id});
     
      });
   },

   search(){
    var  locations = document.getElementById('search_in').value;
    if (locations == "") {
      return false;
    }
    this.woeidData = [];
    this.loadWeather(locations);
    this.$router.push("/search/"+locations);
    this.locations   = [locations]; 
   }
 },

 created() {

    var url =this.$route.name;
  
     if ( url == "Home") {
      var  locations = this.locations;    
      for (var i = 0; i < locations.length; i++) {
        console.log(locations[i]);
        this.loadWeather(locations[i]);
      }

    }

}

}
</script>
