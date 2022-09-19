<script setup>
/* eslint-disable */

import { onMounted, ref } from "vue";
import Current from "./components/CurrentWeather.vue";
import AnimationLoader from "./components/AnimationLoader.vue";
import Forecast from "./components/Forecast.vue";

const API_KEY = "a337a1654ced48cc84170717221204";
const BASE_URL = "https://api.weatherapi.com/v1";

const greetingState = ref();
const permissionState = ref();
const coorState = ref();
const currentWeatherState = ref();
const lastUpdateState = ref();

const setGreetingState = () => {
  const hour = new Date().getHours();

  switch (Math.trunc(hour / 6)) {
    case 0: greetingState.value = 'Sleep'; break;
    case 1: greetingState.value = 'Morning'; break;
    case 2: greetingState.value = 'Afternoon'; break;
    case 3: greetingState.value = 'Evening'; break;
    default: 'Riddance';
  }
}

const fetchCurrentWeatherData = (lat, long) => {
  fetch(`${BASE_URL}/current.json?key=${API_KEY}&q=${lat + "," + long}&aqi=no`)
    .then((data) => data.json())
    .then((obj) => {
      currentWeatherState.value = obj;
      lastUpdateState.value = obj.current.last_updated;
    })
    .catch(console.log);
}

const getCurrentWeather = () => {
  currentWeatherState.value = undefined;
  navigator.geolocation.getCurrentPosition(
    (pos) => { 
      coorState.value = { lat: pos.coords.latitude, long: pos.coords.longitude};

      fetchCurrentWeatherData(pos.coords.latitude, pos.coords.longitude);
    },
    () => console.log
  );
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
  setGreetingState();
});

</script>

<template>
  <header>
    <h3>Good {{ greetingState }}</h3>
  </header>
  <div class="main-container" v-if='permissionState && currentWeatherState'>
    <Current :data="currentWeatherState" />
    <div class="main-right">
      <Forecast v-if="coorState" :data="{ BASE_URL, API_KEY, coorState }" />
    </div>
  </div>
  <AnimationLoader v-else-if="permissionState && !currentWeatherState" />
  <button v-else id='enable-location-button' @click="enableLocationPermission">
    Please enable location
  </button>
  <div class="sticky-container">
    <button id="refresh-button" @click="getCurrentWeather">
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
  grid-template-rows: .3fr 600px .6fr;
  gap: 20px;

  max-width: 100vw;

  place-items: center;
  border-radius: 10px;
}

#app * {
  font-family: 'Montserrat', sans-serif;
}

header {
  display: flex;
  align-items: center;
  justify-content: center;

  font-size: xx-large;
  font-weight: 900;
}

header * {
  margin: 0;
}

.main-container {
  width: 70%;
  height: 100%;
  margin: 10px 0;

  display: inline-grid;
  grid-template-columns: .4fr 1fr;
  gap: 20px;
  justify-items: center;
  align-items: center;
}

.main-right {
  display: flex;
  width: 100%;
}

.sticky-container {
  width: 90%;
  margin-top: 20px;
  display: grid;
  align-content: center;
  justify-items: center;
}

.sticky-container #refresh-button {
  width: 250px;
  display: inline-flex;
  align-items: center;
  justify-content: center;

  font-size: larger;
  font-weight: bold;

  cursor: pointer;
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

.sticky-container h3 {
  margin: 7px 10px;
}

#last-update {
  font-size: 12px;
}

@media screen and (max-width: 600px) {
  .sticky-container #refresh-button {
    width: 90%;
  }
}
</style>
