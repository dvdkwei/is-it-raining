<script setup>/* eslint-disable */

  const props = defineProps({
    componentKey: Number, 
    time: String, 
    icon: String, 
    temperature: Number
  });

</script>
  
<template>
  <div :key=props.componentKey class="flex flex-wrap min-w-[40px] h-[80px] items-center">
    <p class="flex w-full justify-center">{{props.time.split(" ")[1]}}</p>
    <img class="w-full h-auto flex justify-center" alt="icon" :src=props.icon />
    <p class="flex w-full justify-center">{{props.temperature + '°'}}</p>
  </div>
</template>