<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <h2>Make a new product</h2>
    <p>Name:<input type="text" v-model="name"></p>
    <p>Description:<input type="text" v-model="description"></p>
    <p>Price:<input type="text" v-model="price"></p>
    <p>Image URL:<input type="text" v-model="image_url"></p>
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
      name: "",
      description: "",
      price: "",
      image_url: "",
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
        name: this.name,
        description: this.description,
        price: this.price,
        image_url: this.image_url,
      };
      axios.post("/api/products", params).then((response) => {
        console.log(response.data);
        this.products.push(response.data);
      });
    },
  },
};
</script>
