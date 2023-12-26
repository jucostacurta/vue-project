<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";
import API from "./service/api";

import Card from "@/components/Card.vue";

const fetchedInitialData = ref([]);
const term = ref("");
const isLoading = ref(false);

const sortBy = (data) =>
  data.sort((a, b) =>
    a.name.common > b.name.common ? 1 : b.name.common > a.name.common ? -1 : 0
  );

onMounted(async () => {
  isLoading.value = true;
  try {
    await axios.get(API.GET_ALL_COUNTRIES).then((response) => {
      const sortedData = sortBy(response.data);
      fetchedInitialData.value = sortedData;
    });
  } catch (error) {
    console.log(error);
  } finally {
    isLoading.value = false;
  }
});

const searchByName = async () => {
  isLoading.value = true;
  try {
    await axios
      .get(`${API.GET_BY_COUNTRY_NAME}/${term.value}`)
      .then((response) => {
        const sortedData = sortBy(response.data);
        fetchedInitialData.value = sortedData;
      });
  } catch (error) {
    fetchedInitialData.value = [];

    console.log(error);
  } finally {
    isLoading.value = false;
  }
};
</script>

<template>
  <v-container v-if="isLoading">
    <v-row align="center" justify="center">
      <v-progress-circular
        :size="50"
        color="primary"
        class="mt-5"
        indeterminate
      />
    </v-row>
  </v-container>

  <v-container v-else>
    <v-row align="center" justify="center">
      <v-col cols="10" class="mt-2">
        <v-text-field
          :append-icon="term ? 'mdi-filter-check' : 'mdi-filter'"
          :append-inner-icon="'mdi-magnify'"
          v-model="term"
          variant="outlined"
          clear-icon="mdi-close-circle"
          clearable
          label="Busque pelo nome do país"
          type="text"
          @click:append="console.log('open filters')"
          @click:append-inner="searchByName"
          @keydown.enter="searchByName"
        ></v-text-field>
      </v-col>
    </v-row>

    <v-row align="center" justify="center">
      <Card v-for="item in fetchedInitialData" v-bind:key="item" :item="item" />
    </v-row>
    <v-row align="center" justify="center" v-if="fetchedInitialData.length < 1">
      <v-col cols="10" class="mt-2"> Nenhum resultado encontrado. </v-col>
    </v-row>
  </v-container>
</template>
