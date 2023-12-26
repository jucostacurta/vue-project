<template>
  {{ data.map((item) => item.name.common) }}
</template>

<script>
import axios from "axios";
import API from "./service/api";

export default {
  data() {
    return {
      data: [],
    };
  },
  mounted() {
    axios
      .get(API.GET_ALL_COUNTRIES)
      .then((response) => {
        this.data = response.data;
      })
      .catch((error) => {
        console.log(error);
      });
  },
};
</script>
