<template>
  <main >
    <div class="search-box">
      <input type="text" class="search-bar"  placeholder="search..."
      v-model="quary" @keypress="fetchWeather">
    </div>

    <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">

      <div class="location-box" >
        <div class="location">{{weather.name}},{{weather.sys.country}}</div>
        <div class="date">{{dateBuilder()}}</div>
      </div>

      <div class="weather-box">
        <div class="temp">{{Math.round(weather.main.temp)}}℃</div>
        <div class="weather">{{weather.weather[0].main}}</div>
      </div>

    </div>
  </main>
</template>

<script>

export default {
  name:"api",
  data() {
    return {
      api_key:"51c97fcae84c8b0f870f0216ab38670a",
      url_base:"http://api.openweathermap.org/data/2.5/",
      quary:"",
      weather:[]
    }
  },
  methods: {
    fetchWeather(event){
      if(event.key == 'Enter'){
        //Please note that removing the &units=metric entirely will return the temperature as Kelvin.To get Fahrenheit use &units=imperial
        fetch(`${this.url_base}weather?q=${this.quary}&units=metric&APPID=${this.api_key}`)
        .then((res)=>{
          return res.json();
        })
        .then(this.setResult);
      }
      
    },
    setResult(result){
    this.weather = result;
    },
    dateBuilder(){
        let d = new Date();
        let months = [
          "January","February","March","April","May","June","July","August",
          "September","October","November","December"
        ];
        let days = [ 
          "Sunday","Monday","Thuesday","Wednesday","Thursday","Friday","Saturday"
        ]
        let day = days[d.getDay()]
        let date =d.getDate();
        let month = months[d.getMonth()];
        let year = d.getFullYear();

        return `${day} ${date} ${month} ${year}`
    }
  },
  
  
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  
}
</style>
