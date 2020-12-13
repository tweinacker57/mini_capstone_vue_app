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
        <p><strong>Name: </strong><input type="text" v-model="currentProduct.name"></p>
        <p><strong>Description: </strong><input type="text" v-model="currentProduct.description"></p>
        <p><strong>Price: $</strong><input type="text" v-model="currentProduct.price"></p>
        <p><strong>Image: </strong><input type="text" v-model="currentProduct.image_url"></p>
        <p><button v-on:click=updateProduct()>Update Product</button></p>
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
    updateProduct: function () {
      var params = {
        name: this.currentProduct.name,
        description: this.currentProduct.description,
        price: this.currentProduct.price,
        image_url: this.currentProduct.image_url,
      };
      console.log("updating products");
      axios
        .patch("/api/products/" + this.currentProduct.id, params)
        .then((response) => {
          console.log(response.data);
        });
    },
  },
};
</script>
