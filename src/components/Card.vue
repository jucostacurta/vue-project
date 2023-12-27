<script setup>
defineProps(["item"]);
</script>
<template>
  <v-col cols="3">
    <v-card
      :title="item.name.common"
      :subtitle="item.region"
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
    </v-card>
  </v-col>
</template>
