<script setup>/* eslint-disable */
  const props = defineProps({
    data: Object
  });

  const KEY = props.data.time_epoch;
  const TIME = props.data.time.split(' ')[1];
  const ICON = props.data.condition.icon;
  const TEMPERATURE = Math.trunc(props.data.temp_c);

</script>
  
<template>
  <div :key=KEY class="flex flex-wrap min-w-[60px] h-[100px] items-center">
    <p class="flex w-full justify-center">{{TIME}}</p>
    <img class="w-full h-auto flex justify-center" alt="icon" :src=ICON />
    <p class="flex w-full justify-center">{{TEMPERATURE + '°'}}</p>
  </div>
</template>
  
<style>

</style>