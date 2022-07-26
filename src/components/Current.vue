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
      <h2>{{ props.data.location.region }}</h2>
      <h3>{{ props.data.location.country }}</h3>
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
  gap: 5rem;
  grid-template-rows: .3fr .6fr .3fr;
  justify-items: center;
  align-items: top;
  padding: 7px;

  background-color: rgba(27, 27, 27, 0.9);

  border-radius: 20px;
}

.location-text {
  height: 100%;
  width: 100%;

  display: grid;

  justify-items: center;

  border-radius: 10px;
}

.location-text h2,
h3 {
  margin: 0;
}

.location-text h2 {
  font-weight: bolder;
  font-size: xx-large;
}

.location-container img {
  width: 40px;
}

.temperature {
  display: inline-flex;
  justify-items: center;
  align-content: center;
  height: 150px;
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
  align-self: end;
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