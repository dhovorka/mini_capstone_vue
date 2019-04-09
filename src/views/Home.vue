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
    <br />
    <button v-on:click="setSortAttribute('description')">Sort by Description</button>
    <br />
    <button v-on:click="setSortAttribute('name')">Sort by Name</button>
    Search by name or description:
    <input type="text" v-model="searchFilter" />
    <div
      v-for="product in orderBy(filterBy(products, searchFilter, 'name', 'description'), sortAttribute, sortAscending)"
    >
      <h2>Title: {{ product.name }}</h2>
      <img v-bind:src="product.images" v-bind:alt="product.title" />
      <div>
        <button v-on:click="currentProduct = product">More Info</button>
      </div>
      <div v-if="product === currentProduct">
        <p>Name: {{ product.name }}</p>
        <p>Price: {{ product.price }}</p>
        <p>Description: {{ product.description }}</p>
        <div>
          <h4>Edit Product</h4>
          Name:
          <input type="text" v-model="product.name" />
          Price:
          <input type="text" v-model="product.price" />
          Description:
          <input type="text" v-model="product.description" />
          <button v-on:click="updateProduct(product)">update product</button>
          <button v-on:click="destroyProduct(product)">Destroy product</button>
        </div>
      </div>
    </div>
  </div>
</template>

<style></style>

<script>
var axios = require("axios");
import Vue2Filters from "vue2-filters";

export default {
  mixins: [Vue2Filters.mixin],
  data: function() {
    return {
      message: "Welcome to the jungle!",
      name: "Dan",
      products: [],
      newProductName: "",
      newProductPrice: "",
      newProductDescription: "",
      currentProduct: null,
      searchFilter: "",
      sortAttribute: "name",
      sortAscending: 1
    };
  },
  created: function() {
    axios.get("/api/products").then(response => {
      this.products = response.data;
    });
  },
  methods: {
    setSortAttribute: function(inputAttribute) {
      if (this.sortAttribute === inputAttribute) {
        if (this.sortAscending === 1) {
          this.sortAscending = -1;
        } else {
          this.sortAscending = 1;
        }
        this.sortAscending - 1;
      }
      this.sortAttribute = inputAttribute;
    }
  },
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
  },
  updateProduct: function(product) {
    var params = {
      name: product.name,
      price: product.price,
      description: product.description
    };
    axios.patch("/api/products/" + product.id, params).then(response => {
      console.log("Success", response.data);
      product = response.data;
    });
  },
  destroyProduct: function(product) {
    axios.delete("/api/products/" + product.id).then(response => {
      console.log("successfully deleted", response.data);
      var index = this.products.indexOf(product);
      this.products.splice(index, 1);
    });
  }
};
</script>
