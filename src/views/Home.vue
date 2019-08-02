<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <p>Name: <input type="text" v-model="newProductName"></p>
    <p>Description: <input type="text" v-model="newProductDescription"></p>
   <button v-on:click="makeProduct()">Make a new product</button>
    <div v-for="product in products">
      <p>title:{{product.name}}</p>
      <p>image url: {{product.image}}</p>
      <img v-bind:src="product.images_url" v-bind:alt="product.name"/>
      <p><button v-on:click="setProduct(product)">Show more info</button></p>
      <div v-if="currentProduct === product">
        <p>description: {{ product.description }}</p>
        <p>Title: <input type="text" v-model="product.name"></p>
        <p>Description: <input type="text" v-model="product.description"></p>
        <p>Image URL: <input type="text" v-model="product.image"></p>
        <button v-on:click="updateProduct(product)">Update the product</button>
        <hr>
        <button v-on:click="destroyProduct(product)">Destroy the product</button>
        <hr>
      </div>
      <hr>
    </div>
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      message: "Hi!",
      products: [],
      newProductName: "",
      newProductDescription: "",
      currentProduct: {}
    };
  },
  created: function() {
    console.log('hi');
    axios.get("/api/products").then(response => {
      console.log(response.data);
      this.products = response.data;
    });
  },
  methods: {
    makeProduct: function() {
      console.log('make product method');
      //get some data
      var newProduct = {
        name: this.newProductName,
        Description: this.newProductDescription
      };
      //send data to the API
      axios.post('/api/products', newProduct).then(response => {
        console.log('in callback for create');
        console.log(response.data);
        this.products.push(response.data);
      });
    },
    setProduct: function(theProduct) {
      console.log('setting the product');
      console.log(theProduct);
      this.currentProduct = theProduct;
    },
    updateProduct: function(theProduct) {
      console.log('updating product');
      console.log(theProduct);
      axios.patch('/api/products/' + theProduct.id, theProduct).then(response => {
        console.log(response.data);
        theProduct.name = response.data.name;
        theProduct.description = response.data.description;
        theProduct.image = response.data.image;
      });
    },
    destroyProduct: function(theProduct) {
      console.log('destroying product');
      console.log(theProduct);
      axios.delete('/api/products/' + theProduct.id).then(response => {
        console.log(response.data);
        var index = this.products.indexOf(theProduct);
        this.products.splice(index, 1);
      });
    }
  }
};
</script>
