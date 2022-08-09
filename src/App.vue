<script setup>
/* eslint-disable */

import { onMounted, ref } from "vue";
import Current from "./components/CurrentWeather.vue";
import AnimationLoader from "./components/AnimationLoader.vue";

const API_KEY = "a337a1654ced48cc84170717221204";
const BASE_URL = "https://api.weatherapi.com/v1";

const permissionState = ref();
const currentWeatherState = ref();
const lastUpdateState = ref();

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

const queryLocationPermission = () => {
  navigator.permissions.query({ name: 'geolocation' }).then((result) => {
    if (result.state === 'denied') {
      permissionState.value = false;
      return;
    }
    permissionState.value = true;
    getCurrentWeather();
  });
}

const enableLocationPermission = () => {
  permissionState.value = true;
  queryLocationPermission();
}

onMounted(() => {
  queryLocationPermission();
});

</script>

<template>
  <header>
    <h3>weather</h3>
  </header>
  <div class="main-container">
    <Current v-if='permissionState && currentWeatherState' :data="currentWeatherState" />
    <AnimationLoader v-else-if="permissionState && !currentWeatherState">Loading ...</AnimationLoader>
    <button id='enable-location-button' v-else @click="enableLocationPermission">
      Please enable location
    </button>
  </div>
  <div class="sticky-container">
    <button id="refresh-button" @click="refreshCurrentWeather">
      <h3>refresh</h3>&nbsp;&nbsp;
      <img id="refresh" alt="refresh" src="./assets/icons8-refresh-64.png" />
    </button>
    <p id='last-update' v-if="lastUpdateState">
      last updated: {{ lastUpdateState }} 
    </p>
  </div>
</template>

<style>
@import "./assets/base.css";

#app {
  display: grid;
  max-width: 100vw;
  height: 100%;
  justify-items: center;
  align-content: center;
  border-radius: 10px;
}

#app * {
  font-family: 'Montserrat', sans-serif;
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

.sticky-container {
  height: 10vh;
  width: 90%;
  margin-top: 20px;
  display: grid;
  align-content: center;
  justify-items: center;
}

.sticky-container #refresh-button {
  width: 400px;
  display: inline-flex;
  align-items: center;
  justify-content: center;

  font-size: larger;
  font-weight: bold;

  color: aliceblue;
  background: none;
  border: 1px solid rgba(255, 255, 255, .5);
  border-radius: 10px;
  /* width: 1.1rem; */
}

.sticky-container #refresh-button:hover {
  background-color: rgba(0, 0, 0, .6);
}

.sticky-container #refresh {
  width: 1.2rem;
  filter: invert(100%);
}

.sticky-container h3{
  margin: 7px 10px;
}

#last-update {
  font-size: 12px;
}

@media screen and (max-width: 600px) {
  .sticky-container #refresh-button{
    width: 90%;
  }
}
</style>
