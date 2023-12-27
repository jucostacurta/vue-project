<template>
  <v-app>
    <default-bar />
    <Breadcrumb :currentPage="params.country" />

    <default-view />
  </v-app>
</template>

<script setup>
import DefaultBar from "./AppBar.vue";
import DefaultView from "./View.vue";
import Breadcrumb from "@/components/Breadcrumb.vue";

import { useRoute } from "vue-router";

const { params } = useRoute();
</script>
