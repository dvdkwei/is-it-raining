<script setup>/* eslint-disable */
  const props = defineProps({
    componentKey: Number, 
    time: String, 
    icon: String, 
    temperature: Number
  });

  // const KEY = props.data.time_epoch;
  // const TIME = props.data.time.split(' ')[1];
  // const ICON = props.data.condition.icon;
  // const TEMPERATURE = Math.trunc(props.data.temp_c);

</script>
  
<template>
  <div :key=props.componentKey class="flex flex-wrap min-w-[60px] h-[100px] items-center">
    <p class="flex w-full justify-center">{{props.time.split(" ")[1]}}</p>
    <img class="w-full h-auto flex justify-center" alt="icon" :src=props.icon />
    <p class="flex w-full justify-center">{{props.temperature + '°'}}</p>
  </div>
</template>
  
<style>

</style>