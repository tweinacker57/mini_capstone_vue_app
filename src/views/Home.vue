<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <h2>Make a new product</h2>
    <button v-on:click="createProduct()">Make product</button>
    <br>
    <br>
    <br>
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
    createProduct: function () {
      console.log("creating product");
      var params = {
        name: "Desk",
        description: "kjldf;a",
        price: 15.99,
        image_url:
          "https://secure.img1-fg.wfcdn.com/im/27295110/resize-h800-w800%5Ecompr-r85/9789/97898251/Harbaugh+26%2522+Table+Lamp.jpg",
      };
      axios.post("/api/products", params).then((response) => {
        console.log(response.data);
        this.products.push(response.data);
      });
    },
  },
};
</script>
