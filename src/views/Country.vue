<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";
import API from "./service/api";
import { useRoute } from "vue-router";

const country = ref();
const isLoading = ref(false);
const { params } = useRoute();

const searchByCountry = async () => {
  isLoading.value = true;
  try {
    await axios
      .get(`${API.GET_BY_COUNTRY_NAME}/${params.country}`)
      .then((response) => {
        const res = response.data[0];
        country.value = res;
      });
  } catch (error) {
    country.value = [];
    console.log(error);
  } finally {
    isLoading.value = false;
  }
};

onMounted(async () => {
  searchByCountry();
});
</script>

<template>
  <v-container v-if="!country">
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
      <v-card
        :title="country.name.common"
        :subtitle="`${country.continents.join(', ')}`"
        class="mx-lg"
        width="1200"
        variant="text"
      >
        <template v-slot:prepend>
          <v-avatar rounded="0" size="170">
            <v-img :src="country.flags.png" :lazy-src="country.flags.png" />
          </v-avatar>
        </template>

        <v-divider />
        <v-card-text class="text-caption">
          <b>Capital: </b>{{ country.capital.join(", ") }}
        </v-card-text>
        <v-divider />

        <v-card-text class="text-caption pb-1">
          <b>População: </b>{{ country.population }}
        </v-card-text>
        <v-card-text class="text-caption pt-1 pb-1">
          <b>Timezone: </b>{{ country.timezones.join(", ") }}
        </v-card-text>
        <v-card-text class="text-caption pt-1 pb-1" v-if="country.languages">
          <b>Linguas: </b>{{ Object.values(country.languages).join(", ") }}
        </v-card-text>
        <v-card-text class="text-caption pt-1" v-if="country.currencies">
          <b>Moedas: </b>{{ Object.keys(country.currencies).join(", ") }}
        </v-card-text>
        <v-divider />
      </v-card>
    </v-row>
  </v-container>
</template>
