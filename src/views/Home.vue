<template>
  <div class="home">
    <button v-on:click="createProduct()">Create product</button>
    <h1>{{ message }}</h1>
    <p>{{ name }}</p>
    <div v-for="product in products">
      <h2>Title: {{ product.name }}</h2>
      <img v-bind:src="product.images" v-bind:alt="product.title" />
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
      products: []
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
        name: "test name",
        price: "test price",
        description: "test description",
        supplier_id: 1
      };
      axios.post("/api/products", params).then(response => {
        console.log("success", response.data);
      });
    }
  }
};
</script>
