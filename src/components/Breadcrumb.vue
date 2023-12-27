<script setup>
const { currentPage } = defineProps(["currentPage"]);

const breadcrumbList = [
  {
    title: "Página inicial",
    disabled: false,
    href: "/",
  },
  {
    title: currentPage,
    disabled: true,
  },
];
</script>

<template>
  <v-container class="pb-0 mb-0">
    <v-breadcrumbs :items="breadcrumbList">
      <template v-slot:prepend>
        <v-icon size="small" icon="$vuetify"></v-icon>
      </template>
    </v-breadcrumbs>
  </v-container>
</template>
