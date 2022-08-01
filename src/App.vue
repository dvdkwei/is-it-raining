<script setup>
/* eslint-disable */

import { onMounted, ref } from "vue";
import Current from "./components/Current.vue";

const API_KEY = "a337a1654ced48cc84170717221204";
const BASE_URL = "https://api.weatherapi.com/v1";

const permissionState = ref();
const currentWeatherState = ref();
const lastUpdateState = ref();

if(!('geolocation' in navigator)) alert('no navigator');

const fetchCurrentWeatherData = (lat, long) => {
  fetch(BASE_URL + `/current.json?key=${API_KEY}&q=${lat + "," + long}&aqi=no`)
    .then((data) => data.json())
    .then((obj) => {
      currentWeatherState.value = obj;
      lastUpdateState.value = obj.current.last_updated;
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

const enableLocationPermission = () => {
  permissionState.value = true;
  refreshCurrentWeather();
}

onMounted(() => {
  navigator.permissions.query({name:'geolocation'}).then((result) => {
    if(result.state === 'denied'){
      permissionState.value = false;
      return;
    }
    permissionState.value = true;
    getCurrentWeather();
  });
});

</script>

<template>
  <header>
    <h3>weather</h3>
  </header>
  <div class="main-container">
    <Current v-if='permissionState && currentWeatherState' :data="currentWeatherState" />
    <h3 v-else-if="permissionState && !currentWeatherState">Loading ...</h3>
    <button 
      id='enable-location-button' 
      v-else
      @click="enableLocationPermission"
    >
      Please enable location
    </button>
  </div>
  <footer @click="refreshCurrentWeather">
    <h3>refresh</h3>&nbsp;&nbsp;
    <img id="refresh" alt="refresh" src="./assets/icons8-refresh-64.png" />
  </footer>
  <p id='last-update' v-if="lastUpdateState">
    last updated: {{ lastUpdateState }}
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
