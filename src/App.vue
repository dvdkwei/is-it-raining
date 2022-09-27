<script setup>
/* eslint-disable */

import { onMounted, ref, onUpdated } from "vue";
import Current from "./components/CurrentWeather.vue";
import AnimationLoader from "./components/AnimationLoader.vue";
import ForecastContainer from "./containers/ForecastContainer.vue";
import NewsContainer from "./containers/NewsContainer.vue";
import ActivitiesContainer from "./containers/ActivitiesContainer.vue";

const API_KEY = import.meta.env.VITE_WEATHER_API_KEY;
const BASE_URL = import.meta.env.VITE_BASE_WEATHER_URL;

const greeting = ref();
const permission = ref(true);
const coordinates = ref();
const currentWeather = ref();
const lastUpdate = ref();

const setGreetingState = () => {
  const hour = new Date().getHours();

  switch (Math.trunc(hour / 6)) {
    case 0: greeting.value = 'Sleep'; break;
    case 1: greeting.value = 'Morning'; break;
    case 2: greeting.value = 'Afternoon'; break;
    case 3: greeting.value = 'Evening'; break;
    default: 'Riddance';
  }
}

const fetchCurrentWeatherData = (lat, long) => {
  fetch(`${BASE_URL}/current.json?key=${API_KEY}&q=${lat + "," + long}&aqi=no`)
    .then((data) => data.json())
    .then((obj) => {
      currentWeather.value = obj;
      lastUpdate.value = obj.current.last_updated;
    })
    .catch(console.log);
}

const getCurrentWeather = () => {
  currentWeather.value = undefined;
  navigator.geolocation.getCurrentPosition(
    (pos) => { 
      permission.value = true;
      coordinates.value = { lat: pos.coords.latitude, long: pos.coords.longitude};
      fetchCurrentWeatherData(pos.coords.latitude, pos.coords.longitude);
    },
    () => {
     permission.value = false; 
    }
  );
}

const setPermissionToTrue = () => {
  // permission.value = true;
  getCurrentWeather();
}

onMounted(() => {
  setGreetingState();
  getCurrentWeather();

  if(!localStorage.getItem('date')){
    localStorage.setItem('date', Date());
  }
});

</script>

<template>
  <header>
    <h3 id="greeting">Good {{ greeting }}</h3>
  </header>
  <div class="main-container" v-if='permission && currentWeather'>
    <div class="main-left">
      <Current 
        :tempCelcius="currentWeather.current.temp_c"
        :tempFarenheit="currentWeather.current.temp_f"
        :weatherText="currentWeather.current.condition.text" 
        :weatherIcon="`http:${currentWeather.current.condition.icon}`"
        :region="currentWeather.location.region"
        :country="currentWeather.location.country"
      />
      <ForecastContainer 
        v-if="coordinates" 
        :BASE_URL="BASE_URL"
        :API_KEY="API_KEY"
        :coordinates="coordinates"
      />
    </div>
    <div class="main-right">
      <NewsContainer />
      <ActivitiesContainer />
    </div>
  </div>
  <AnimationLoader v-else-if="permission && !currentWeather" />
  <h1 v-else class="text-xl" id='enable-location-button' @click="setPermissionToTrue">
    Please enable location in your browser settings
  </h1>
  <div class="sticky-container">
    <button id="refresh-button" @click="getCurrentWeather">
      <h3>refresh</h3>&nbsp;&nbsp;
      <img id="refresh" alt="refresh" src="./assets/icons8-refresh-64.png" />
    </button>
    <p id='last-update' v-if="lastUpdate">
      last updated: {{ lastUpdate }}
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
  max-width:70%;
  height: 100%;
  margin: 10px 0;

  display: inline-grid;
  grid-template-columns: .3fr 1fr;
  gap: 10px;
  justify-items: center;
}

.main-left{
  display: grid;
  grid-template-rows: 1fr auto;
  gap: 10px;
}

.main-right {
  display: grid;
  grid-template-rows: 1fr .8fr;
  width: 100%;
  max-width: 100%;
  gap: 10px;
  align-items: start;
  animation: fade-in 1.5s;
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

@keyframes fade-in{
  from{
    opacity: 10%;
  }
  to{
    opacity: 100%;
  }
}

@media screen and (max-width: 600px) {
  .sticky-container #refresh-button {
    width: 90%;
  }
}
</style>
