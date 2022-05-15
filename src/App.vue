<template>
<div id="home">
  <div class="box" >
    <div class="dateYear">{{db.year}}</div>
    <div class="date">{{db.day}},{{db.date}} {{db.month}}</div>

      <div
          class="card"
           v-for="weather in cityMap.values()"
           v-bind:style="{
             backgroundImage: `url(${getImgUrl(weather.weather[0].main)})`
           }"
      >
          <div class="location"  >{{weather.name}}</div>
        <div class="weather_box" v-if="typeof  weather.main!='undefined'">
          <div class="weather_tem">{{ Math.round(weather.main.temp) }}℃</div>
          <div class="weather_weather"  >{{ weather.weather[0].main }}</div>
        </div>

       </div>

  </div>

</div>
</template>

<script>
import {ref} from "vue";
export default {
  name:"home",
  data(){
    return{
      api_key:"5a306bc73f0cb9922c8fd498d92bcce6",
      url_base:"https://api.openweathermap.org/data/2.5/",
      cityMap: new Map(),
      db: {},
      clear:'https://user-images.githubusercontent.com/104249805/168463327-e7e11959-a501-4a73-a59a-9c85dad5fb1c.jpg',
      clouds:'https://user-images.githubusercontent.com/104249805/168463720-f83d91b3-dbe6-49a7-9f4c-46c29ddba785.jpg',
      rain:'https://user-images.githubusercontent.com/104249805/168463320-f977fe7e-79a1-40db-aabb-fb812acac067.jpg',
      imgMap: new Map()
    }
  },
  mounted() {
    this.imgMap.set("Clear",this.clear)
    this.imgMap.set("Rain",this.rain)
    this.imgMap.set("Clouds",this.clouds)
    this.db=this.dataBuilder()
    this.fetchWeather('chengdu')
    this.fetchWeather('shanghai')
    this.fetchWeather('bazhong')
    this.fetchWeather('ganzhou')
  },
  methods:{
    async fetchWeather(A){
      fetch(`${this.url_base}weather?q=${A}&units=metric&APPID=${this.api_key}`)
            .then (res=>{return res.json();})
            .then( res=>{
              this.cityMap.set(A,res)
              console.log(this.cityMap)
            })
    },

    getImgUrl(key){
      if(this.imgMap.has(key))return this.imgMap.get(key)
      else return this.clouds
    },

    dataBuilder(){
      let d=new Date();
      let months=["January","February","March","April","May","June","July","August","September","October","November","December"];
      let days=["Sunday","Monday","Tuesday","Wednesday","Thursday","Fridy","Saturday"]
      let day=days[d.getDay()];
      let date=d.getDate();
      let month=months[d.getMonth()];
      let year=d.getFullYear();
      return {
         month:month,date:date,day:day,year:year
      };
    }
  }
}
</script>

<style lang="scss">
html,
body{
  height: 100%;
  -webkit-user-select: none;
  -webkit-touch-callout: none;
  -webkit-tap-highlight-color: transparent;

}
body{
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen,
    Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
}
#app {
  height: 100%;
}

#home{
  width: 100%;
  height: 100%;
  position: relative;
  display: flex;
  justify-content: center;
   }
.box{
  position: absolute;
  top: 50px;
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;

}

.card{
  //-webkit-text-stroke-width: 1px;
  //-webkit-text-stroke-color: black;
  display: flex;
  position: relative;
  max-width: 500px;
  max-height: 100px;
  width: 90%;
  height: 20vw;
  padding: 5px 0;
  margin: 20px 0;
  box-shadow: -6px -6px 20px rgba(255,255,255,1),
              -6px -6px 20px rgba(255,255,255,0.5),
              inset 6px 6px 20px rgba(255,255,255,0.1),
              6px 6px 20px rgba(0,0,0,0.15),
}
.card::after{
  filter: blur(2px);
}

.dateYear{
  color: #5a84a2;
  font-size:2.5rem;
  font-weight: 600;
  text-align: center;
  text-shadow: 6px 6px 20px rgba(0,0,0,0.15);
}
.date{
  color: #5a84a2;
  font-size:30px;
  font-weight: 500;
  text-align: center;
  text-shadow: 6px 6px 20px rgba(0,0,0,0.15);
}

.location{
  position: absolute;
  left: 1rem;
  color: white;
  font-size:1.7rem;
  font-weight: 600;
  margin-top: 20px;
  text-shadow: 6px 6px 20px rgba(0,0,0,0.15);
}
.weather_box{
  position: absolute;
  left: 65%;
  //margin-top: 1rem;
  color: white;
  font-size: 1.7rem;
  text-shadow: 6px 6px 20px rgba(0,0,0,0.15);

}
.weather_tem{

}
.weather_weather{
}
</style>
