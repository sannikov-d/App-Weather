<script>
export default {
  name: "WeatherBlock",
  props:{
    Temperature: {
      type: Number,
      default: 0
    },
    CityName: {
      type: String,
      default: ''
    }
  }
}
</script>

<template>
  <div class="main-panel__weather-block">
    <h3 v-show="CityName != ''" class="main-panel__city-name">{{ CityName }}</h3>
    <p v-show="CityName != ''" class="main-panel__temperature">Temperature: {{ Temperature }} C</p>
    <p v-show="CityName == ''" class="main-panel__temperature">City not found</p>
  </div>
</template>

<style lang="scss" src="../assets/css/WeatherBlock.scss"></style>