<script setup>
/* eslint-disable */

import { onMounted, ref } from "vue";
import Current from "./components/Current.vue";

const API_KEY = "a337a1654ced48cc84170717221204";
const BASE_URL = "https://api.weatherapi.com/v1";

const currentWeatherState = ref();
const lastUpdate = ref();

const fetchCurrentWeatherData = (lat, long) => {
  fetch(BASE_URL + `/current.json?key=${API_KEY}&q=${lat + "," + long}&aqi=no`)
    .then((data) => data.json())
    .then((obj) => {
      currentWeatherState.value = obj;
      lastUpdate.value = obj.current.last_updated;
    })
    .catch(console.log);
}

const getCurrentWeather = () => {
  navigator.geolocation.getCurrentPosition(
    (pos) => { fetchCurrentWeatherData(pos.coords.latitude, pos.coords.longitude) },
    () => console.log
  );
}

const refreshCurrentWeather = () => {
  currentWeatherState.value = undefined;
  getCurrentWeather();
}

onMounted(() => getCurrentWeather());

</script>

<template>
  <header>
    <h3>weather</h3>
  </header>
  <div class="main-container">
    <Current v-if='currentWeatherState' :data="currentWeatherState" />
    <h3 v-else>Loading ...</h3>
  </div>
  <footer @click="refreshCurrentWeather">
    <h3>refresh</h3>&nbsp;&nbsp;
    <img id="refresh" alt="refresh" src="./assets/icons8-refresh-64.png" />
  </footer>
  <p id='last-update' v-if="lastUpdate">
    last updated: {{ lastUpdate }}
  </p>
</template>

<style>
@import "./assets/base.css";

#app {
  display: grid;
  max-width: 100vw;
  justify-items: center;
  border-radius: 10px;
}

header {
  height: 15vh;

  display: flex;
  align-items: center;
  justify-content: center;

  font-size: xx-large;
  font-weight: 900;
}

.main-container {
  height: 65vh;
  width: 100%;
  margin: 10px 0;

  display: flex;
  align-items: center;
  justify-content: center;
}

footer {
  height: 5vh;
  width: 90%;
  margin-top: 20px;
  display: flex;
  align-items: center;
  justify-content: center;

  border: 1px white solid;
  border-radius: 10px;
}

footer h2 {
  font-weight: bolder;
}

footer #refresh {
  width: 1.1rem;
  filter: invert(100%);
}

#last-update {
  font-size: 12px;
}

@media screen and (max-width: 600px) {
  footer {
    width: 90%;
  }
}
</style>
