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
        <!-- CREATE PRODUCT -->
        <v-card-item
          ><v-text-field
            v-model="newProduct.upc"
            label="Enter UPC"
            variant="outlined"
            class="pa-2"
          ></v-text-field
        ></v-card-item>
        <v-card-item
          ><v-text-field
            v-model="newProduct.location"
            label="Enter Location"
            variant="outlined"
            class="pa-2"
          ></v-text-field
        ></v-card-item>
        <v-container>
          <v-row class="justify-center pa-3">
            <v-btn
              @click="createProduct()"
              color="white"
              class="bg-primary justify-center"
              width="80%"
              >Create</v-btn
            >
          </v-row>
        </v-container>
      </v-card>
      <!-- SEARCH PRODUCT -->
      <v-card
        v-else-if="!update"
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
        <v-card-item v-if="product"
          ><div>Location: {{ product.location }}</div></v-card-item
        >
        <v-card-item v-if="product">
          <v-btn @click="deleteProduct(product.upc)" color="red">
            Delete
          </v-btn>
          <v-btn @click="toggleUpdate()" class="ml-5" color="green">
            Update
          </v-btn>
        </v-card-item>
        <v-container>
          <v-row class="justify-center pa-3">
            <v-btn
              @click="getProduct(upc)"
              color="white"
              class="bg-primary justify-center"
              width="80%"
              >Search</v-btn
            >
          </v-row>
        </v-container>
      </v-card>
      <!-- UPDATE PRODUCT -->
      <v-card
        v-if="update"
        title="Update Product"
        class="text-primary border df"
        width="20%"
      >
        <v-card-item
          ><v-text-field
            v-model="updateLocation"
            label="Enter New Location"
            variant="outlined"
            class="pa-2"
          ></v-text-field
        ></v-card-item>
        <v-card-item>
          <v-btn @click="toggleCancel()" color="red"> cancel </v-btn>
          <v-btn @click="updateProduct(product.upc)" class="ml-5" color="green">
            Confirm Update
          </v-btn>
        </v-card-item>
      </v-card>
    </v-row>
    <v-row class="justify-center" v-for="product in products" :key="product.id">
      <div class="pa-5">UPC: {{ product.upc }}</div>
      <div class="pa-5">Location: {{ product.location }}</div>
    </v-row>
  </v-container>
</template>

<script setup>
import { ref, onMounted, watch } from "vue";
import axios from "axios";

const isCreate = ref(true);
const update = ref(false);
const products = ref([]);
const upc = ref("");
const updateLocation = ref('')
const product = ref("");
const newProduct = ref({
  upc: "",
  location: "",
});

const toggleisCreate = () => {
  isCreate.value = !isCreate.value;
  update.value = false;
};

const toggleUpdate = () => {
  update.value = true;
};

const toggleCancel = () => {
  update.value = false
  updateLocation.value = ''
}

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
    .then((response) => (product.value = response.data.product));
};

// Create a new Product
const createProduct = () => {
  axios
    .post("https://products-api-putj.onrender.com/products", newProduct.value)
    .then((response) => console.log(response.data.product))
};

//Update a Product
const updateProduct = (upc) => {
  axios
    .patch(`https://products-api-putj.onrender.com/products/${upc}`, {
      location: updateLocation.value
    })
    .then(update.value = false);
};
// Delete a Product
const deleteProduct = (upc) => {
  axios
    .delete(`https://products-api-putj.onrender.com/products/${upc}`)
    .then((response) => (product.value = response.data.product));
};

// Watch for changes in the products array
watch(products, async (newProducts) => {
  // Do something when the products array changes
  console.log("Products array has changed:", newProducts);
  
  // Refetch the products from the server
  try {
    const response = await axios.get("https://products-api-putj.onrender.com/products");
    products.value = response.data.products;
  } catch (error) {
    console.error("Error fetching products:", error);
  }
});
</script>

<style lang="scss" scoped></style>
