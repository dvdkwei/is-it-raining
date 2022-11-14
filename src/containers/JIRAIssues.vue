<script setup>/*eslint-disable*/

import { onMounted, ref } from 'vue';

const props = defineProps({
  username: String,
});

const issues = ref();

const myHeaders = new Headers();
myHeaders.append("Accept", "application/json");
myHeaders.append("Authorization", "Basic cm9ja2V0LmNoYXQ6aEdYbDNOZnAhLz9LNzJOV1Ql");
myHeaders.append("Cookie", "JSESSIONID=77058467EF6DF6E62FC3BA55B4C4623E; atlassian.xsrf.token=BN23-G35O-ESHF-L3EA|41ee66b0425d341d1e697f734081a59f92eb9164|lin");
myHeaders.append('Authorization', 'Basic ' + ('wlec:wlec').toString('base64'));

const requestOptions = {
  method: 'GET',
  headers: myHeaders,
  redirect: 'follow'
};

const fetchIssues = async () => {
  fetch('http://jira.wlec.ag:20304/rest/api/latest/search?jql=assignee=david.weinardy+order+by+status&fields=id,key,updated,status,priority,creator,summary', requestOptions)
    .then(res => res.json())
    .then(data => {console.log(data); issues.value = data.issues;})
    .catch(err => console.log(err))
}

onMounted(() => {
  fetchIssues();
})

</script>

<template>
  <div class="news-container flex w-full h-full bg-[#1b1b1b]/[.9] rounded-2xl">
    <div class="flex items-center justify-center w-full h-full">JIRA Activities</div>
    <p v-if="issues">{{issues}}</p>
  </div>
</template>