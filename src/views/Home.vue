<template>
  <div class="home">
    <h1>New Product</h1>
    <div>
      <div>
        Name: <input v-model="newProductName">
      </div>
      <div>
        Description: <input v-model="newProductDescription">
      </div>
      <div>
        Price: <input v-model="newProductPrice">
      </div>
      <div>
        Image URL: <input v-model="newProductImageUrl">
      </div>
      <button v-on:click="createProduct()">Create</button>
    </div>

    <h1>All Products</h1>
    <div v-for="product in products">
    <h2>{{ product.name }}</h2>
    <img v-bind:src="product.image_url" v-bind:alt="product.name">
    <p>Price: {{ product.formatted.price }}</p>
    </div>
  </div>
</template>

<style>
  
  img{
    width: 200px;
  }
</style>

<script>
  var axios = require('axios');

export default {
  data: function() {
    return {
      products: [],
      newProductName: "",
      newProductDescription: "",
      newProductPrice: "",
      newProductImageUrl: ""
    };
  },
  created: function() {
    axios.get("/api/products")
      .then(response => {
        this.products = response.data;
      });
  },
  methods: {
    createProduct: function() {
          console.log("Create the Product...");
          var params = {
                        name: this.newProductName,
                        description: this.newProductDescription,
                        price: this.newProductPrice,
                        image_url: this.newProductImageUrl
                        };
          axios.post("/api/products", params)
            .then(response => {
              console.log("Success", response.data);
              this.products.push(response.data);
            });
        }
      }
    };
</script>


