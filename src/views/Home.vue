<template>
  <div class="home">
    <h2>Products</h2>
    <div>
      Name:
      <input type="text" v-model="newProduct.name" />
    </div>
    <div>
      Price:
      <input type="text" v-model="newProduct.price" />
    </div>
    <div>
      Description:
      <input type="text" v-model="newProduct.description" />
    </div>
    <div>
      Image Url:
      <input type="text" v-model="newProduct.image_url" />
    </div>
    <button v-on:click="createProduct()">Create Product</button>
    <div v-for="product in products" v-bind:key="product.id">
      <h3>Name: {{ product.name }}</h3>
      <img v-bind:src="product.image_url" />
      <h4>Price: {{ product.price }}</h4>
      <h4>Description: {{ product.description }}</h4>
      <div>
        <button v-on:click="showProduct(product)">More Info</button>
      </div>
    </div>
    <dialog id="product-details">
      <form method="dialog">
        <h1>Product info</h1>
        <p>
          Name:
          <input type="text" v-model="currentProduct.name" />
        </p>
        <p>
          Price:
          <input type="text" v-model="currentProduct.price" />
        </p>
        <p>
          Description:
          <input type="text" v-model="currentProduct.description" />
        </p>
        <button v-on:click="updateProduct(currentProduct)">Update</button>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>
<style>
#product-details {
  width: 300px;
}
</style>
<script>
import axios from "axios";

export default {
  data: function () {
    return {
      products: [],
      currentProduct: {},
      newProduct: {
        name: "",
        price: "",
        description: "",
        image_url: "",
      },
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
    createProduct: function () {
      var params = {
        name: this.newProduct.name,
        price: this.newProduct.price,
        description: this.newProduct.description,
        image_url: this.newProduct.image_url,
      };
      axios.post("http://localhost:3000/products", params).then((response) => {
        this.products.push(response.data);
        params = {
          name: "",
          price: "",
          description: "",
          image_url: "",
        };
      });
    },
    showProduct: function (product) {
      console.log("Show Product", product);
      this.currentProduct = product;
      document.querySelector("#product-details").showModal();
    },
    updateProduct: function (currentProduct) {
      console.log("Update product", currentProduct);
      var params = {
        name: currentProduct.name,
        price: currentProduct.price,
        description: currentProduct.description,
      };
      axios.patch(`http://localhost:3000/products/${currentProduct.id}`, params).then((response) => {
        console.log("This is update", response.data);
      });
    },
  },
};
</script>
