<script setup>/*eslint-disable*/
import { onMounted, ref } from "vue";

const BASE_NEWS_URL = import.meta.env.VITE_BASE_NEWS_URL;
const NEWS_API_KEY = import.meta.env.VITE_NEWS_API_KEY;

const isToday = ref();
const newsData = ref();

const setIsToday = () => {
  const today = Date();
  isToday.value = (localStorage.getItem('date') === today);
}

const fetchNewsData = () => {
  if(isToday) return;

  fetch(`${BASE_NEWS_URL}${NEWS_API_KEY}&countries=de&date=`)
}

onMounted(() => {
  setIsToday();
})
</script>

<template>
  <div class="news-container flex w-full h-full bg-[#1b1b1b]/[.9] rounded-2xl ">
    <div class="flex items-center justify-center w-full h-full">Latest News</div>
  </div>
</template>