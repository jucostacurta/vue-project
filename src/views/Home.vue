<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";
import API from "./service/api";

import Card from "@/components/Card.vue";

const fetchedInitialData = ref([]);
const term = ref("");
const isLoading = ref(false);
const dialog = ref(false);
const continents = ref("All");

const sortBy = (data) =>
  data.sort((a, b) =>
    a.name.common > b.name.common ? 1 : b.name.common > a.name.common ? -1 : 0
  );

const searchAll = async () => {
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
};

const searchByName = async () => {
  isLoading.value = true;
  if (!term.value) return searchAll();
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

const searchByContinent = async () => {
  isLoading.value = true;
  if (!continents.value || continents.value === "All") return searchAll();
  try {
    await axios
      .get(`${API.GET_BY_CONTINENT}/${continents.value}`)
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

onMounted(async () => {
  searchAll();
});
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
          :append-icon="
            term || continents !== 'All' ? 'mdi-filter-check' : 'mdi-filter'
          "
          :append-inner-icon="'mdi-magnify'"
          v-model="term"
          variant="outlined"
          clear-icon="mdi-close-circle"
          clearable
          label="Busque pelo nome do país"
          type="text"
          @click:append="dialog = true"
          @click:append-inner="searchByName"
          @keydown.enter="searchByName"
        />
      </v-col>
    </v-row>

    <v-row justify="center">
      <v-dialog
        v-model="dialog"
        transition="dialog-bottom-transition"
        width="500"
      >
        <v-card>
          <v-toolbar dark color="primary">
            <v-btn icon dark @click="dialog = false">
              <v-icon>mdi-close</v-icon>
            </v-btn>
            <v-toolbar-title>Filtro</v-toolbar-title>
            <v-spacer></v-spacer>
            <v-toolbar-items>
              <v-btn
                variant="text"
                @click="
                  dialog = false;
                  searchByContinent();
                "
              >
                Salvar
              </v-btn>
            </v-toolbar-items>
          </v-toolbar>
          <v-radio-group v-model="continents">
            <v-container>
              <v-row>
                <v-col cols="6">
                  <v-radio color="primary" label="All" value="All" />
                </v-col>
                <v-col cols="6">
                  <v-radio color="primary" label="Asia" value="Asia" />
                </v-col>
                <v-col cols="6">
                  <v-radio color="primary" label="Africa" value="Africa" />
                </v-col>
                <v-col cols="6">
                  <v-radio
                    color="primary"
                    label="North America"
                    value="North America"
                  />
                </v-col>
                <v-col cols="6">
                  <v-radio
                    color="primary"
                    label="South America"
                    value="South America"
                  />
                </v-col>
                <v-col cols="6">
                  <v-radio
                    color="primary"
                    label="Antarctica"
                    value="Antarctica"
                  />
                </v-col>
                <v-col cols="6">
                  <v-radio color="primary" label="Europe" value="Europe" />
                </v-col>
                <v-col cols="6">
                  <v-radio
                    color="primary"
                    label="Australia"
                    value="Australia"
                  />
                </v-col>
              </v-row>
            </v-container>
          </v-radio-group>
        </v-card>
      </v-dialog>
    </v-row>

    <v-row align="center" justify="center">
      <Card v-for="item in fetchedInitialData" v-bind:key="item" :item="item" />
    </v-row>
    <v-row align="center" justify="center" v-if="fetchedInitialData.length < 1">
      <v-col cols="10" class="mt-2">Nenhum resultado encontrado.</v-col>
    </v-row>
  </v-container>
</template>
