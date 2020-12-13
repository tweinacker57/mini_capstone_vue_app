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
      {{ product.id }}
      {{ product.name }}
      <p><button v-on:click="showProduct(product)">Show more info</button></p>
    <p><img v-bind:src= "product.image_url" v-bind:alt= "product.title"></p>
    </div>
    <dialog id="product-details">
      <form method="dialog">
        <h3>Hello</h3>
        <p><strong>Name: </strong>{{ currentProduct.name }}</p>
        <p><strong>Description: </strong>{{ currentProduct.description }}</p>
        <p><strong>Price: $</strong>{{ currentProduct.price }}</p>
        <p><strong>Image: </strong>{{ currentProduct.image_url }}</p>
        <button>Close</button>
      </form>
    </dialog>
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
      currentProduct: {},
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
    showProduct: function (theProduct) {
      console.log(theProduct);
      this.currentProduct = theProduct;
      console.log("showing product");
      document.querySelector("#product-details").showModal();
    },
  },
};
</script>
