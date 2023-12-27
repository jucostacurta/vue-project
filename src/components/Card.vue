<script setup>
defineProps(["item"]);
</script>
<template>
  <v-col cols="4">
    <v-card
      :title="item.name.common"
      :subtitle="`${item.continents.join(', ')}`"
      class="mx-auto"
      max-width="344"
      link
    >
      <template v-slot:prepend>
        <v-avatar rounded="0" size="70">
          <v-img
            :src="item.flags.png"
            :lazy-src="item.flags.png"
            width="70"
          ></v-img>
        </v-avatar>
      </template>

      <v-divider />
      <v-card-text class="text-caption pb-1">
        <b>População: </b>{{ item.population }}
      </v-card-text>
      <v-card-text class="text-caption pt-1 pb-1">
        <b>Timezone: </b>{{ item.timezones.join(", ") }}
      </v-card-text>
      <v-card-text class="text-caption pt-1" v-if="item.languages">
        <b>Linguas: </b>{{ Object.values(item.languages).join(", ") }}
      </v-card-text>
      <v-divider />

      <v-card-actions>
        <v-btn variant="text" color="teal-accent-4" @click="reveal = true">
          Saiba mais
        </v-btn>
      </v-card-actions>
    </v-card>
  </v-col>
</template>
