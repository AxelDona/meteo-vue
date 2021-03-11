<template>
  <div id="app" v-bind:class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
   <main>
     <div class="search-ctnr">
       <input
           type="text"
           name="" id="searchbar"
           class="search-bar"
           v-bind:placeholder="placeholder"
           v-model="query"
           @keypress="fetchWeather"
       />
     </div>

     <div class="meteo-wrap" v-if="typeof weather.main != 'undefined'">
       <div class="loc-ctnr">
         <div class="loc">{{ weather.name }}, {{ weather.sys.country }}</div>
         <div class="date">{{ creerDate() }}</div>
       </div>
       <div class="meteo-ctnr">
         <div class="temp" v-bind:class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
           {{ Math.round(weather.main.temp) }}<span id="celsius">°C</span>
         </div>
         <img
             v-bind:src="'http://openweathermap.org/img/wn/' + weather.weather[0].icon + '@2x.png'"
             v-bind:alt="statutMeteo()"
             class="icon"
         >
         <div class="meteo">{{ statutMeteo() }}</div>
         <div class="meteo-desc">{{ weather.weather[0].description }}</div>
         <br />
         <div class="feels-like more-info">
           Température ressentie : {{ Math.round(weather.main.feels_like) }}°C
         </div>
         <div class="hum more-info">
           Humidité : {{ weather.main.humidity }} %
         </div>
       </div>
     </div>
   </main>
  </div>
</template>

<script>

export default {
  name: 'app',
  data () {
    return {
      api_key: '871fb26a1930703f6ef6a98885938954',
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: '',
      weather: {},
      placeholder: 'Recherche...',
    }
  },
  methods: {
    fetchWeather (e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}&lang=fr`)
        .then(res=> {
          return res.json();
        }).then(this.setResults);
        this.query = '';
      }
    },
    setResults (results) {
      this.weather = results;
    },
    creerDate () {
      let d = new Date();
      let tousMois = ["janvier", "février", "mars", "avril", "mai", "juin", "juillet", "août",
         "septembre", "octobre", "novembre", "décembre"];
      let tousJours = ["Dimanche", "Lundi", "Mardi", "Mercredi", "Jeudi", "Vendredi", "Samedi"];
      let jour = tousJours[d.getDay()];
      let date = d.getDate();
      let mois = tousMois[d.getMonth()];
      let annee = d.getFullYear();

      return `${jour} ${date} ${mois} ${annee}`;
    },
    statutMeteo () {
      if (this.weather.weather[0].main == 'Dust'){
        return 'Tempête de sable';
      } else if (this.weather.weather[0].main == 'Rain'){
        return 'Pluie';
      } else if (this.weather.weather[0].main == 'Clouds'){
        return 'Nuages';
      } else if (this.weather.weather[0].main == 'Clear'){
        return 'Temps clair';
      }
    }
  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body{
  font-family: 'Montserrat', sans-serif;
}

#app {
  background-image : url('./assets/cold-bg.jpg');
  background-size : cover;
  background-position : bottom;
  transition : 400ms;
}

#app.warm {
  background-image : url('./assets/warm-bg.jpg');
}

main {
  min-height : 100vh;
  padding : 25px;

  background-image : linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}

.search-ctnr {
  width : 100%;
  margin-bottom : 30px;
}

.search-ctnr .search-bar{
  display : block;
  width : 100%;
  padding : 15px;

  color : #313131;
  font-size : 20px;

  appearance : none;
  border : none;
  outline : none;
  background : none;

  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color : rgba(255, 255, 255, 0.5);
  border-radius : 0px 16px 0px 16px;
  transition : 400ms;
}

.search-ctnr .search-bar:focus{
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color : rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}

.loc-ctnr .loc {
  color : #ffffff;
  font-size : 32px;
  font-weight : 500;
  text-align : center;
  text-shadow : 1px 3px rgba(0, 0, 0, 0.25);
}

.loc-ctnr .date{
  color : #ffffff;
  font-size : 20px;
  font-weight : 300;
  font-style: italic;
  text-align: center;
}

.meteo-ctnr{
  text-align: center;
}

.meteo-ctnr .temp{
  display: inline-block;
  padding: 10px 25px;
  color : #90e0ef;
  font-size : 102px;
  font-weight : 800;
  text-shadow:
      8px 8px #00b4d8,
      20px 20px #0077b6;

  background-color : rgba(0, 0, 0, 0.25);
  border : 1px solid #ffffff;
  border-radius: 2px;
  margin : 30px 0px;
  -webkit-text-stroke-width: 1px;
  transition-duration: 400ms;
}

.meteo-ctnr .temp.warm{
  color : #e85d04;
  text-shadow:
      8px 8px #d00000,
      20px 20px #6a040f;
}

.meteo-ctnr #celsius{
  -webkit-text-fill-color: rgba(0, 0, 0, 0.25);
  -webkit-text-stroke-width: 1px;
  -webkit-text-stroke-color: #dddddd;
  text-shadow: none;
  padding-left: 15px;
}

.meteo-ctnr .icon{
  display: block;
  margin: 0 auto;
}

.meteo-ctnr .meteo{
  color : #ffffff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.meteo-ctnr .meteo-desc{
  color : #ffffff;
  font-size: 18px;
  font-weight: 300;
  text-shadow: 1px 1px rgba(0, 0, 0, 0.25);
  text-transform: capitalize;
}

.meteo-ctnr .more-info{
  color : #ffffff;
  font-size: 18px;
  font-weight: 300;
  text-shadow: 1px 1px rgba(0, 0, 0, 0.25);
}


</style>
