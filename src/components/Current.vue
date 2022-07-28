<script setup>/* eslint-disable */

import { ref } from "vue";

const props = defineProps({
  data: Object,
});

const state = ref({
  tempValue: props.data.current.temp_c,
  tempDegree: 'c', 
  weatherText: props.data.current.condition.text, 
  weatherIcon: 'http:' + props.data.current.condition.icon
});

const toggleTemperature = () => {
  if (state.value.tempDegree === 'c') {
    state.value.tempValue = props.data.current.temp_f;
    state.value.tempDegree = 'f';
    return;
  }
  state.value.tempValue = props.data.current.temp_c;
  state.value.tempDegree = 'c';
}


</script>

<template>
  <div class="current">
    <div class="location-text">
      <img id='location-icon' src="../assets/icons8-location-48.png" alt="location"/>
      <h3>{{ props.data.location.region + ', ' +props.data.location.country }}</h3>
    </div>
    <div class="temperature" @click="toggleTemperature">
      <p class="value">{{ state.tempValue }}</p>
      <p class="degree">{{ ' °' + state.tempDegree }}</p>
    </div>
    <div class="weather">
      <p class="wdesc">{{ state.weatherText }}</p>
      <img class="wicon" :src=state.weatherIcon alt="icon"/>
    </div>
  </div>
</template>

<style scoped>
.current {
  height: 60vh;
  width: 60vw;

  display: grid;
  gap: 4rem;
  grid-template-rows: .3fr 1fr .2fr;
  justify-items: center;
  align-items: top;
  padding: 7px;

  background-color: rgba(27, 27, 27, 0.9);

  border-radius: 20px;
}

.location-text {
  height: 100%;
  width: 100%;

  display: inline-flex;
  column-gap: 2em;
  justify-content: center;
  align-items: center;

  border-radius: 10px;
}

.location-text h3 {
  margin: 0;
}

.location-text h3 {
  font-weight: bolder;
  font-size: x-large;
}

.location-text #location-icon {
  width: 1.7rem;
  filter: invert(100%);
}

.location-container img {
  width: 40px;
}

.temperature {
  display: inline-flex;
  justify-items: center;
  align-items: center;
  height: 100%;
}

.temperature:hover{
  cursor: pointer;
}

.value {
  font-size: 8em;
  margin: 0;
}

.degree {
  font-size: 3rem;
  margin: 0;
  margin-top: 40px;
  /* align-self: start; */
}

.weather{
  width: 100%;
  display: inline-flex;
  justify-content: space-around;
  align-items: center;
}

.weather .wdesc {
  font-size: x-large;
}

@media screen and (max-width: 600px) {
  .current{
    width: 90vw;
  }
}
</style>