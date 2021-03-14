<template>
  <div id="app" v-bind:class="typeof weather.main != 'undefined' ? weather.weather[0].main.toLowerCase() : ''">
    <main>
      <div class="search-ctnr">
        <label for="searchbar"></label>
        <input
            type="text"
            name="searchbar"
            id="searchbar"
            class="search-bar"
            v-bind:placeholder="placeholder"
            v-model="query"
            @keypress="fetchWeather"
        />
      </div>
      <div class="meteo-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="loc-ctnr">
          <div class="loc">
            <span class="city">{{ weather.name }}</span>
            <span class="country">, {{ weather.sys.country }}</span>
          </div>
          <div class="date">{{ creerDate() }}</div>
        </div>
        <hr/>
        <div class="meteo-ctnr">
          <div class="temp">
            <div>
              <span id="number" v-bind:class="typeof weather.main != 'undefined' ? temperatureTier() : ''">{{ Math.round(weather.main.temp) }}</span>
              <span id="celsius">°C</span>
            </div>
          </div>
          <div class="meteo">{{ weather.weather[0].description }}</div>
        </div>
        <div class="more-info">
          <table>
            <tr>
              <td class="info-value"><span class="value">{{ Math.round(weather.main.feels_like) }}</span><span class="unit">°C</span></td>
              <td class="info-desc">Température ressentie</td>
            </tr>
            <tr>
              <td class="info-value"><span class="value">{{ weather.main.humidity }}</span><span class="unit"> %</span></td>
              <td class="info-desc">Humidité</td>
            </tr>
            <tr>
              <td class="info-value"><span class="value">{{ weather.main.pressure }}</span><span class="unit"> hPa</span></td>
              <td class="info-desc">Pression atmoshpérique</td>
            </tr>
            <tr>
              <td class="info-value"><span class="value">{{ Math.round(weather.wind.speed*3.6) }}</span><span class="unit"> km/h</span> </td>
              <td class="info-desc">Vitesse du vent</td>
            </tr>
          </table>
        </div>
      </div>
    </main>
  </div>
</template>

<script>

export default {
  name: 'app',
  data() {
    return {
      api_key: '871fb26a1930703f6ef6a98885938954',
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: '',
      weather: {},
      placeholder: 'Recherche...'
    }
  },
  methods: {
    tellWeather(){
      fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}&lang=fr`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
    },
    fetchWeather(e) {
      if (e.key == "Enter") {
        this.tellWeather();
        document.getElementById('searchbar').blur();
        this.tellWeather();
        this.query = '';
      }
    },
    setResults(results) {
      this.weather = results;
    },
    creerDate() {
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
    temperatureTier() {
      if (this.weather.main.temp < 0){
        return 'very-cold'
      } else if (this.weather.main.temp < 10){
        return 'cold'
      } else if (this.weather.main.temp < 20){
        return 'medium'
      } else if (this.weather.main.temp < 30){
        return 'hot'
      } else if (this.weather.main.temp < 40){
        return 'very-hot'
      } else if (this.weather.main.temp < 50){
        return 'very-very-hot'
      } else{
        return 'default'
      }
    }
  }
}
</script>

<style lang="scss" rel="stylesheet/scss">

@import "assets/css/style.scss";

</style>
