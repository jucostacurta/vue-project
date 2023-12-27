<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";
import API from "./service/api";

import Card from "@/components/Card.vue";

const fetchedData = ref([]);

onMounted(() => {
  axios
    .get(API.GET_ALL_COUNTRIES)
    .then((response) => {
      const sortedData = response.data.sort((a, b) =>
        a.name.common > b.name.common
          ? 1
          : b.name.common > a.name.common
          ? -1
          : 0
      );
      fetchedData.value = sortedData;
    })
    .catch((error) => {
      console.log(error);
    });

  return { fetchedData };
});
</script>

<template>
  <v-container>
    <v-row align="center" justify="center">
      <Card v-for="item in fetchedData" v-bind:key="item" :item="item" />
    </v-row>
  </v-container>
</template>
