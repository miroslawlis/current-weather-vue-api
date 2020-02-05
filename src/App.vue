<template>
  <div id="app" v-bind:class="this.bg_class">
    <main>
      <a id="home" href="http://lkspromna.pl">Powrót na stronę główną</a>

      <div id="input-wraper">
        <input
          type="text"
          placeholder="Podaj lokalizacje"
          v-model="query"
          @keypress="getDataFromAPI"
        />
      </div>
      <button id="button" v-on:click="getDataFromAPI">Wyszukaj</button>

      <div class="location-wrapper" v-if="typeof weather.main != 'undefined'">
        <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
        <div class="date"></div>
      </div>

      <div class="weather-wrapper" v-if="typeof weather.main != 'undefined'">
        <div class="temp">{{ Math.round(weather.main.temp) }} °c</div>
        <img
          id="icon_img"
          :src="'https://openweathermap.org/img/wn/' + weather.weather[0].icon + '@2x.png'"
          alt="current weather"
        />
        <div class="weather">{{ weather.weather[0].description }}</div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      query: "",
      weather: {},
      icon_url: "",
      bg_class: ""
    };
  },
  methods: {
    getDataFromAPI: function(e) {
      let queryDefault = this.query ? this.query : "Falęcice";

      if (queryDefault == "Falęcice") {
        // no key/mouse event then show default location
        fetch(`http://lkspromna.pl/api/weather.php?loc=${queryDefault}`)
          .then(res => {
            return res.json();
          })
          .then(this.updateWeatherObj);
      } else if (e.key == "Enter" || e.type == "click") {
        fetch(`http://lkspromna.pl/api/weather.php?loc=${queryDefault}`)
          .then(res => {
            return res.json();
          })
          .then(this.updateWeatherObj);
      }
    },
    updateWeatherObj(results) {
      this.weather = results;
      this.bg_set(this.weather.weather[0].main);
    },
    bg_set(weather_main) {
      // update background winth proper class based on data from API (main field)
      // weather_main = weather.weather[0].main

      weather_main = weather_main.toLowerCase();

      switch (weather_main) {
        case "clear sky":
          this.bg_class = "sunny";
          break;
        case "clear":
          this.bg_class = "sunny";
          break;
        case "mist":
          this.bg_class = "mist";
          break;
        case "few clouds":
          this.bg_class = "clouds1";
          break;
        case "scattered clouds":
          this.bg_class = "clouds2";
          break;
        case "broken clouds":
          this.bg_class = "clouds3";
          break;
        case "shower rain":
          this.bg_class = "rain2";
          break;
        case "rain":
          this.bg_class = "rain1";
          break;
        case "thunderstorm":
          this.bg_class = "thunderstorm";
          break;
        case "snow":
          this.bg_class = "snow";
          break;

        default:
          this.bg_class = "sunny";
      }
    }
  },
  beforeMount() {
    this.getDataFromAPI();
  }
};
</script>

<style>
</style>
