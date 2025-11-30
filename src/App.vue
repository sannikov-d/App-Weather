<template>
  <div id="app">
    <div class="main-panel">
      <header class="main-panel__header">
        <h1>App Weather</h1>
        <hr class="main-panel__divider"/>
      </header>
      <div class="main-panel__body">
        <input v-model="currentCity" @keydown.enter="getWeather(currentCity)" class="city-input" type="text"
               placeholder="Enter city">
        <WeatherBlock v-show="currentTemp != 0" :Temperature="currentTemp" :CityName="resultCityName"/>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import WeatherBlock from "@/components/WeatherBlock.vue";

export default {
  name: 'App',
  components: {WeatherBlock},
  data() {
    return {
      currentCity: "",
      resultCityName: "",
      currentTemp: 0,
    }
  },
  methods: {
    async getWeather(cityName) {
      try {
        const res = await axios.get(`http://api.openweathermap.org/geo/1.0/direct?q=${cityName}&limit=1&appid=e7d5d748d554330620be283d9cc5aae6`)
        if (res.status === 200) {
          const lat = res.data.length != 0 ? res.data[0].lat : 0;
          const lon = res.data.length != 0 ? res.data[0].lon : 0;
          this.resultCityName = res.data.length != 0 ? res.data[0].name : '';

          await this.getWeatherByCord(lat, lon);
        }
      } catch (error) {
        console.log(error);
      }
    },
    async getWeatherByCord(lat, lon) {
      try {
        const res = await axios.get(`https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&units=metric&appid=e7d5d748d554330620be283d9cc5aae6`)
        if (res.status === 200) {
          this.currentTemp = Math.ceil(res.data.main.temp);
        }
      }
      catch (error) {
        console.log(error);
      }
    }
  }
}
</script>

<style lang="scss" src="./assets/css/Main.scss"></style>
