<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div v-for="product in products">
      {{ product.name }}
    <p><img v-bind:src= "product.image_url" v-bind:alt= "product.title"></p>
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
        this.products = response.data;
      });
    },
  },
};
</script>
