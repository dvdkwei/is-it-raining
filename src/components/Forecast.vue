<script setup>/* eslint-disable */

import {onMounted, ref} from "vue";

const props = defineProps({data: Object});

const forecastState = ref();

const fetchCurrentState = () => {
  fetch(`${props.data.BASE_URL}/forecast.json?key=${props.data.API_KEY}&q=${props.data.coorState.lat + ',' + props.data.coorState.long}&days=2`)
    .then(x => x.json())
    .then(x => forecastState.value = x);
}

onMounted(() => {
  fetchCurrentState();
})

</script>

<template>
  <div v-if="forecastState" class="forecast-container grid grid-rows-[.1fr_1fr] w-full h-[150px] bg-red rounded-2xl">
    {{forecastState.forecast.forecastday.date}}
  </div>
</template>

<style>
  .forecast-container {
    background-color: rgba(27, 27, 27, .9);
  }
</style>