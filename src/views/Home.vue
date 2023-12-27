<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";
import API from "./service/api";

import Card from "@/components/Card.vue";

const fetchedInitialData = ref([]);
const term = ref("");
const isLoading = ref(false);
const dialog = ref(false);
const continents = ref([]);

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
          :append-icon="term || continents ? 'mdi-filter-check' : 'mdi-filter'"
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
        fullscreen
        :scrim="false"
        transition="dialog-bottom-transition"
        v-modal="dialog"
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
          <v-list lines="two" subheader>
            <v-list-item
              title="Content filtering"
              subtitle="Set the content filtering level to restrict apps that can be downloaded"
            >
              <v-radio-group v-model="continents">
                <v-radio color="primary" label="All" value="All" hide-details />
                <v-radio
                  color="primary"
                  label="Asia"
                  value="Asia"
                  hide-details
                />
                <v-radio
                  color="primary"
                  label="Africa"
                  value="Africa"
                  hide-details
                />
                <v-radio
                  color="primary"
                  label="North America"
                  value="North America"
                  hide-details
                />
                <v-radio
                  color="primary"
                  label="South America"
                  value="South America"
                  hide-details
                />
                <v-radio
                  color="primary"
                  label="Antarctica"
                  value="Antarctica"
                  hide-details
                />
                <v-radio
                  color="primary"
                  label="Europe"
                  value="Europe"
                  hide-details
                />
                <v-radio
                  color="primary"
                  label="Australia"
                  value="Australia"
                  hide-details
                />
              </v-radio-group>
            </v-list-item>
          </v-list>
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
