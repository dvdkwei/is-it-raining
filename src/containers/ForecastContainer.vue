<script setup>/* eslint-disable */

import { onMounted, ref, h } from "vue";
import Forecast from "../components/Forecast.vue";

const props = defineProps({
  BASE_URL: String,
  API_KEY: String,
  coordinates: Object
});

const forecast = ref();

const fetchForecasts = () => {
  fetch(`${props.BASE_URL}/forecast.json?key=${props.API_KEY}&q=${props.coordinates.lat + ',' + props.coordinates.long}&days=2`)
    .then(x => x.json())
    .then(x => forecast.value = x);
}

const getForecastArray = (arr) => {
  let hour = new Date().getHours();
  let today = arr.forecast.forecastday[0].hour;
  let tomorrow = arr.forecast.forecastday[1].hour;

  today = today.filter(x => {
    let time = x.time.split(' ')[1];
    return time.match(/[1-2][0-9]|[1-9]/ig) > hour
  });

  return today.concat(tomorrow).slice(0, 24);
}

onMounted(() => {
  fetchForecasts();
})

</script>

<template>
  <div v-if="forecast" class="relative forecasts-container flex gap-x-4 pl-2 min-w-full max-w-full h-[150px] items-center rounded-2xl">
    <template v-for="forecastElement in getForecastArray(forecast)">
      <Forecast :componentKey="forecastElement.time_epoch" :time="forecastElement.time"
        :icon="forecastElement.condition.icon" :temperature="Math.trunc(forecastElement.temp_c)" />
    </template>
    <div 
      class="flex sticky right-0 px-8 top-0 h-full bg-gradient-to-r from-transparent to-black items-center"
    >
      <img class="arrow ml-6 w-10 h-4" alt="slide" src="../assets/chevron-right.png"/>
    </div>
  </div>
</template>

<style lang="scss">
.forecasts-container {
  background-color: rgba(27, 27, 27, .9);
  overflow: auto;
  scrollbar-width: none;
}

.forecasts-container::-webkit-scrollbar {
  height: 0;
}
</style>