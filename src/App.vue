<script setup>
/* eslint-disable */

import { reactive } from "vue";
import Current from "./components/Current.vue";

const API_KEY = "a337a1654ced48cc84170717221204";
const BASE_URL = "https://api.weatherapi.com/v1";

let currentWeatherData = reactive({});

navigator.geolocation.getCurrentPosition(
    (pos) => {
      let currentLatitude = pos.coords.latitude;
      let currentLongitude = pos.coords.longitude;

      if (currentLatitude && currentLongitude) {
        fetch(
            BASE_URL +
            `/current.json?key=${API_KEY}&q=${
                currentLatitude + "," + currentLongitude
            }&aqi=no`
        )
            .then((data) => data.json())
            .then((obj) => {
              currentWeatherData = obj;
            })
            .catch(console.log);
      }
    },
    () => console.log
);
</script>

<template>
  <header>
    <h2>Is It Raining?</h2>
  </header>
  <div class="main-container">
    <Current
        class="current"
        v-if = currentWeatherData.value
        :data="currentWeatherData.value"
    />
  </div>
  <footer>
    footer
  </footer>
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
  height: 20vh;

  display: flex;
  align-items: center;
  justify-content: center;

  font-size: xx-large;
  font-weight: 900;
}

.main-container {
  height: 60vh;
  width: 100%;
  margin: 10px 0;

  display: flex;
  align-items: center;
  justify-content: center;
}

footer {
  height: 10vh;
  width: 80%;
  margin-top: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 1px white solid;
  border-radius: 10px;
}

@media screen and (max-width: 600px) {
  footer{
    width: 90%;
  }
}
</style>
