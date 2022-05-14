<template>
<div id="home">
  <div class="box" >
    <div class="date">{{dataBuilder()}}</div>
    <div class="card" v-on:click="fetchWeather('chengdu')" >
      <div class="location" >{{weather.name}}</div>
      <div class="weather_box" v-if="typeof  weather.main!='undefined'">
        <div class="weather_tem">{{ Math.round(weather.main.temp) }}</div>
        <div class="weather_weather">{{ weather.weather[0].main }}</div>
      </div>
    </div>
    <div class="card" v-on:click="fetchWeather('shanghai')">
      <div class="location" >{{weather.name}}</div>
      <div class="weather_box" v-if="typeof  weather.main!='undefined'">
        <div class="weather_tem">{{ Math.round(weather.main.temp) }}</div>
        <div class="weather_weather">{{ weather.weather[0].main }}</div>
      </div>
    </div>
    <div class="card">3</div>
    <div class="card">4</div>
  </div>

</div>
</template>

<script>
export default {
  name:"home",
  data(){
    return{
      api_key:"5a306bc73f0cb9922c8fd498d92bcce6",
      url_base:"https://api.openweathermap.org/data/2.5/",
      weather:{}
    }
  },
  mounted() {

  },
  methods:{
    async fetchWeather(A){
      fetch(`${this.url_base}weather?q=${A}&units=metric&APPID=${this.api_key}`)
            .then (res=>{return res.json();})
            .then( res=>{return this.weather =res} )
    },

    dataBuilder(){
      let d=new Date();
      let months=["1月","2月","3月","4月","5月","6月","7月","8月","9月","10月","11月","12月"];
      let days=["星期一","星期二","星期三","星期四","星期五","星期六","星期日"]
      let day=days[d.getDay()];
      let date=d.getDate();
      let month=months[d.getMonth()];
      let year=d.getFullYear();
      return `${year}年 ${month} ${date}日 ${day} `;
    }
  }
}
</script>

<style lang="scss">
html,
body{
  height: 100%;
  //border: 3px solid black;
}
#app {
  height: 100%;
  background-color: rgb(87, 232, 219);
}

#home{
  width: 100%;
  height: 100%;
  position: relative;
  display: flex;
  justify-content: center;
  border: 3px solid red;
}
.box{
  position: absolute;
  top: 50px;
  width: 90%;
  display: flex;
  flex-direction: column;
  align-items: center;
  border: 3px solid yellow;
}
.card{
  display: flex;
  position: relative;
  max-width: 500px;
  max-height: 100px;
  width: 80%;
  height: 20vw;
  border: 3px solid pink;
  padding: 5px 0;
  margin: 20px 0;
}
.date{
  color: black;
  font-size:24px;
  font-weight: 300;
  text-align: center;
  box-shadow: 1px 2px rgba(0,0,0,0.5);
}

.location{
  position: absolute;
  left: 10px;
  color: black;
  font-size:32px;
  font-weight: 500;
  margin-top: 10px;
}
.weather_box{
  position: absolute;
  right:20px;
  color: black;
  font-size: 30px;

}
.weather_tem{

}
.weather_weather{
}
</style>
