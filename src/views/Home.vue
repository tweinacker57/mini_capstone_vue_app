<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div v-for="product in products">
      {{ product.title }}
    </div>
  </div>
</template>
<style>
</style>
<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Products",
      products: [],
    };
  },
  created: function () {
    this.productsIndex();
  },
  methods: {
    productsIndex: function () {
      axios.get("/api/products").then((response) => {
        console.log(response.data);
        this.recipes = response.data;
      });
    },
  },
};
</script>
