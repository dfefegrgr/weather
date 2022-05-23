<template>
<div id="home">
  <div class="box" >
    <h2 class="date">{{db.year}}年{{db.month}}{{db.date}}日{{db.day}}</h2>
    <h2>今日天气</h2>
    <div class="currentWeather">
      <div class="card" v-for="weather in cityMap.values()">
      <div class="weather_box" v-if="typeof  weather.main!='undefined'">
        <div>{{cityNameMap.get(weather.name)}}</div>
        <div>{{ weather.weather[0].description }}</div>
        <div>{{ Math.round(weather.main.temp) }}℃</div>
      </div>
      </div>
    </div>

    <h2>未来5日预测</h2>
    <div class="forecast" v-for="loc in locationMap.values()">
      <div class="location">{{mappingMap.get(loc.lat+","+loc.lon)}}</div>
      <div class="card2"  v-for="x in 5" v-if="typeof loc.daily!='undefined'" >
        <div class="">
          {{loc.daily[x].weather[0].description}}
        </div>
        <div>
          {{Math.round(loc.daily[x].temp.day)}}℃
        </div>

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
      locationMap:new Map(),
      mappingMap: new Map(),
      cityNameMap:new Map([
          ['Chengdu','成都'],
          ['Shanghai','上海'],
          ['Bazhong','巴中'],
          ['Ganzhou','赣州'],
          ])
    }
  },
  mounted() {
    this.db=this.dataBuilder()
    this.fetchWeather('chengdu')
    this.fetchWeather('shanghai')
    this.fetchWeather('bazhong')
    this.fetchWeather('ganzhou')
    //chengdu
    this.fetchForecast(['30.39','104.04'])
    //shanghai
    this.fetchForecast(['31.22','121.48'])
    //bazhong
    this.fetchForecast(['31.85','106.754'])
    //ganzhou
    this.fetchForecast(['25.83','114.93'])
    this.mappingMap.set('30.39,104.04','成都')
    this.mappingMap.set('31.22,121.48','上海')
    this.mappingMap.set('31.85,106.754','巴中')
    this.mappingMap.set('25.83,114.93','赣州')
  },
  methods:{
    fetchWeather(A){
      fetch(`${this.url_base}weather?q=${A}&lang=zh_cn&units=metric&APPID=${this.api_key}`)
            .then (res=>{return res.json();})
            .then( res=>{
              this.cityMap.set(A,res)
            })
    },

    fetchForecast(A){
      fetch(`${this.url_base}onecall?lat=${A[0]}&lon=${A[1]}&lang=zh_cn&units=metric&appid=${this.api_key}`)
        .then(res=>{return res.json();})
        .then(res=>{
          this.locationMap.set(A,res);
          console.log(this.locationMap)})
    },

    dataBuilder(){
      let d=new Date();
      let months=["1月","2月","3月","4月","5月","6月","7月","8月","9月","10月","11月","12月"];
      let days=["星期一","星期二","星期三","星期四","星期五","星期六","星期日"]
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
 font: 12px/18px Simsun,Helvetica,Arial,sans-serif;
}
#app {
  height: 100%;
  font-size: 14px;
  color: #344665;
  line-height: 25px;
}

#home{
  width: 100%;
  height: 100%;
  position: relative;
  display: flex;
  justify-content: center;
   }
.box{
  padding: 2rem 9%;
  position: relative;
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
}
h2{
  font-size: 20px;
}
.currentWeather{
  display: flex;
  flex-direction: row;
  font-size: 14px;
  color: #344665;
  line-height: 25px;
}
.forecast{
  display: flex;
  font-size: 14px;
  color: #344665;
  line-height: 25px;
}
.location{
  display: flex;
  flex-direction: column;
}
.card2{
  display: flex;
  flex-direction: column;
  width: 60px;
  height: 70px;
  text-align: center;
}

.weather_box{

}
.card{
  width: 60px;
  height: 80px;

}

</style>
