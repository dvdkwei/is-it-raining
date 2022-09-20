<script setup>/* eslint-disable */

import { onMounted, ref, h } from "vue";
import ForecastComponent from "./ForecastComponent.vue";

const props = defineProps({ data: Object });

const forecastState = ref();

const fetchCurrentState = () => {
  fetch(`${props.data.BASE_URL}/forecast.json?key=${props.data.API_KEY}&q=${props.data.coorState.lat + ',' + props.data.coorState.long}&days=2`)
    .then(x => x.json())
    .then(x => forecastState.value = x);
}

const getForecastArray = (arr) => {
  let hour = new Date().getHours();
  let today = arr.forecast.forecastday[0].hour;
  let tomorrow = arr.forecast.forecastday[1].hour;

  today = today.filter(x => {
    let time = x.time.split(' ')[1];
    return time.match(/[1-2][0-9]|[1-9]/ig) > hour
  });

  console.log(today.concat(tomorrow).slice(0, 24)[0]);
  
  return today.concat(tomorrow).slice(0, 24);
}

const getForecastComponent = (element) => {
  if (!forecastState) return h('p', 'Service not available');

  return (
    h('div', {key: element.time_epoch}, { class: 'flex justify-center' }, [
      h('p', element.time.split(' ')[1]),
      h('img', { alt: 'weather', src: element.condition.icon }),
      h('p', Math.trunc(element.temp_c))
    ])
  );
}

onMounted(() => {
  fetchCurrentState();
})

</script>

<template>
  <div v-if="forecastState" class="forecasts-container flex w-full max-w-[500px] h-[150px] overflow-x-auto items-center rounded-2xl">
    <template class="" v-for="x in getForecastArray(forecastState)">
      <ForecastComponent :data="x" />
    </template>
    <!-- <ForecastComponent :data="getForecastArray(forecastState)[0]" /> -->
    
  </div>
</template>

<style>
.forecasts-container {
  background-color: rgba(27, 27, 27, .9);
}
</style>