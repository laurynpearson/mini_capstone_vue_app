<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <p>Name: <input type="text" v-model="newProductName"></p>
    <p>Description: <input type="text" v-model="newProductDescription"></p>
   <button v-on:click="makeProduct()">Make a new product</button>
    <div v-for="product in products">
      <p>title:{{product.name}}</p>
    <!--   <p>image url: {{product.images_url}}</p> -->
      <img v-bind:src="product.images_url" v-bind:alt="product.name"/>
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
      newProductDescription: ""
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
    }
  }
};
</script>
