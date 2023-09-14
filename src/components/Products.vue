<template>
  <v-container class="border">
    <v-row
      class="justify-center text-h3 font-weight-black border text-primary pa-5"
    >
      Products-Map
    </v-row>
    <v-row class="border pa-3 justify-center">
      <v-btn
        @click="toggleisCreate()"
        color="white"
        class="bg-primary justify-center"
        >{{ isCreate ? "Search" : "Create" }}</v-btn
      >
    </v-row>
    <v-row class="border justify-center pa-5">
      <v-card
        v-if="isCreate"
        title="Create a Product"
        class="text-primary border df"
        width="20%"
      >
        <v-card-item
          ><v-text-field
            label="Enter UPC"
            variant="outlined"
            class="pa-2"
          ></v-text-field
        ></v-card-item>
        <v-card-item
          ><v-text-field
            label="Enter Location"
            variant="outlined"
            class="pa-2"
          ></v-text-field
        ></v-card-item>
        <v-container>
          <v-row class="justify-center pa-3">
            <v-btn color="white" class="bg-primary justify-center" width="80%"
              >Create</v-btn
            >
          </v-row>
        </v-container>
      </v-card>
      <v-card
        v-else
        title="Search for a Product"
        class="text-primary border df"
        width="20%"
      >
        <v-card-item
          ><v-text-field
            v-model="upc"
            label="Enter UPC"
            variant="outlined"
            class="pa-2"
          ></v-text-field
        ></v-card-item>
        <v-card-item
          ><div>Location: {{ product.location }}</div></v-card-item
        >
        <v-container>
          <v-row class="justify-center pa-3">
            <v-btn @click="getProduct(upc)" color="white" class="bg-primary justify-center" width="80%"
              >Search</v-btn
            >
          </v-row>
        </v-container>
      </v-card>
    </v-row>
    <v-row class="justify-center" v-for="product in products" :key="product.id">
      <div class="pa-5">UPC: {{ product.upc }}</div>
      <div class="pa-5">Location: {{ product.location }}</div>
    </v-row>
  </v-container>
</template>

<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";

const isCreate = ref(true);
const products = ref([]);
const upc = ref("");
const product = ref("");

const toggleisCreate = () => {
  isCreate.value = !isCreate.value;
};

// Get All Products when app Mount
onMounted(() => {
  axios
    .get("https://products-api-putj.onrender.com/products")
    .then((response) => (products.value = response.data.products));
});

// Get a product by UPC
const getProduct = (upc) => {
  axios
    .get(`https://products-api-putj.onrender.com/products/${upc}`)
    .then((response) => (product.value) = response.data.product);
};
</script>

<style lang="scss" scoped></style>
