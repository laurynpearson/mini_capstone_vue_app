<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div v-for="product in products">
      <p>title:{{product.name}}</p>
      <p>image url: {{product.image_url}}</p>
      <img v-bind:src="product.image_url" v-bind:alt="product.name"/>
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
      products: []
    };
  },
  created: function() {
    console.log('hi');
    axios.get("/api/products").then(response => {
      console.log(response.data);
      this.products = response.data;
    });
  },
  methods: {}
};
</script>
