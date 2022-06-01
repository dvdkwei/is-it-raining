<script setup>/* eslint-disable */

import { ref } from "vue";
import Temperature from "./Temperature.vue";

const props = defineProps({
  data: Object,
});

const state = ref({
  tempValue: props.data.current.temp_c,
  tempDegree: 'c'
});

const toggleTemperature = () => {
  if (state.value.tempDegree === 'c') {
    state.value.tempValue = props.data.current.temp_f;
    state.value.tempDegree = 'f';
    console.log(state.value.tempValue)
    return;
  }
  state.value.tempValue = props.data.current.temp_c;
  state.value.tempDegree = 'c';

  console.log(state.value.tempValue)
}


</script>

<template>
  <div class="current">
    <div class="location-text">
      <h2>{{ props.data.location.region }}</h2>
      <h3>{{ props.data.location.country }}</h3>
    </div>
    <Temperature :val="state.tempValue" :degree="state.tempDegree" @click="toggleTemperature" />
  </div>
</template>

<style scoped>
.current {
  height: 60vh;
  width: 90%;

  display: grid;
  justify-items: center;
  padding: 10px;

  background-color: rgba(255, 255, 255, .15);

  border-radius: 20px;
}

.location-text {
  border: 1px solid white;

  height: fit-content;
  width: 90%;

  display: grid;

  justify-items: center;
  padding: 10px;

  border-radius: 10px;
}

.location-text h2,
h3 {
  margin: 0;
}

.location-text h2 {
  font-weight: bolder;
  font-size: x-large;
}

.location-container img {
  width: 40px;
}
</style>