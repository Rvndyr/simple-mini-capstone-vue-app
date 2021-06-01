<template>
  <div class="home">
    <h2>Products</h2>
    <div v-for="product in products" v-bind:key="product.id">
      <h4>Name: {{ product.name }}</h4>
      <h4>Price: {{ product.price }}</h4>
    </div>
  </div>
</template>
<style></style>
<script>
import axios from "axios";

export default {
  data: function () {
    return {
      products: [],
    };
  },
  created: function () {
    this.indexProducts();
  },
  methods: {
    indexProducts: function () {
      axios.get("http://localhost:3000/products").then((response) => {
        this.products = response.data;
        console.log(this.products);
      });
    },
  },
};
</script>
