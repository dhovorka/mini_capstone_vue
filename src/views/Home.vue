<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <p>{{ name }}</p>
    <div>
      Name:
      <input type="text" v-model="newProductName" />
      Price:
      <input type="text" v-model="newProductPrice" />
      Description:
      <input type="text" v-model="newProductDescription" />
    </div>
    <button v-on:click="createProduct()">Create product</button>
    <div v-for="product in products">
      <h2>Title: {{ product.name }}</h2>
      <!-- <img v-bind:src="product.images" v-bind:alt="product.title" /> -->
      <div>
        <button v-on:click="currentProduct = product">More Info</button>
      </div>
      <div v-if="product === currentProduct">
        <p>Name: {{ product.name }}</p>
        <p>Price: {{ product.price }}</p>
        <p>Description: {{ product.description }}</p>
      </div>
    </div>
  </div>
</template>

<style></style>

<script>
var axios = require("axios");

export default {
  data: function() {
    return {
      message: "Welcome to Vue.js!",
      name: "Dan",
      products: [],
      newProductName: "",
      newProductPrice: "",
      newProductDescription: "",
      currentProduct: null
    };
  },
  created: function() {
    axios.get("/api/products").then(response => {
      this.products = response.data;
    });
  },
  methods: {
    createProduct: function() {
      console.log("create a product...");
      var params = {
        name: this.newProductName,
        price: this.newProductPrice,
        description: this.newProductDescription
      };
      axios.post("/api/products", params).then(response => {
        console.log("success", response.data);
      });
    }
  }
};
</script>
