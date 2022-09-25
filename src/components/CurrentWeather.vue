<script setup>/* eslint-disable */

import { ref } from "vue";

const props = defineProps({
  tempCelcius: Number, 
  tempFarenheit: Number,
  weatherText: String, 
  weatherIcon: String, 
  region: String, 
  country: String,
});

const tempValue = ref(props.tempCelcius);
const tempDegree = ref('c');

const toggleTemperature = () => {
  if (tempDegree.value === 'c') {
    tempValue.value = props.tempFarenheit;
    tempDegree.value = 'f';
    return;
  }
  tempValue.value= props.tempCelcius;
  tempDegree.value = 'c';
}

</script>

<template>
  <div class="current">
    <div class="location-text">
      <img id='location-icon' src="../assets/icons8-location-48.png" alt="location"/>
      <h3>{{ props.region + ', ' + props.country }}</h3>
    </div>
    <div class="temperature" @click="toggleTemperature">
      <p class="value">{{ tempValue }}</p>
      <p class="degree">{{ ' °' + tempDegree }}</p>
    </div>
    <div class="weather">
      <p class="wdesc">{{ props.weatherText }}</p>
      <img class="wicon" :src=props.weatherIcon alt="icon"/>
    </div>
  </div>
</template>

<style scoped>
.current {
  height: 100%;
  width: 100%;
  min-width: 220px;

  display: grid;
  gap: 4rem;
  grid-template-rows: .3fr 1fr .2fr;
  justify-items: center;
  align-items: top;
  /* padding: 7px; */

  animation: fade-in 1.5s;


  background-color: rgba(27, 27, 27, 0.9);

  border-radius: 16px;
}

.location-text {
  height: 100%;
  width: 100%;

  display: inline-flex;
  column-gap: 1em;
  justify-content: center;
  align-items: center;

  border-radius: 10px;
}

.location-text h3 {
  margin: 0;
  font-weight: bolder;
  font-size: 16px;
}

.location-text #location-icon {
  width: 1.1rem;
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
  font-size: 5em;
  margin: 0;
}

.degree {
  font-size: 2rem;
  margin: 0;
  margin-top: 40px;
  /* align-self: start; */
}

.weather{
  width: 90%;
  display: inline-flex;
  justify-content: space-around;
  align-items: center;
  margin-bottom: 10px;
}

.weather .wdesc {
  font-size: medium;
}

.weather .wicon{
  width: 42px;
}

@media screen and (max-width: 600px) {
  .current{
    width: 100%;
  }
}
</style>