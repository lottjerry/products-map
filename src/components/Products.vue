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
            label="Enter UPC"
            variant="outlined"
            class="pa-2"
          ></v-text-field
        ></v-card-item>
        <v-container>
          <v-row class="justify-center pa-3">
            <v-btn color="white" class="bg-primary justify-center" width="80%"
              >Search</v-btn
            >
          </v-row>
        </v-container>
      </v-card>
    </v-row>
  </v-container>
</template>

<script setup>
import { ref } from "vue";

const isCreate = ref(true);

const toggleisCreate = () => {
  isCreate.value = !isCreate.value;
};
</script>

<style lang="scss" scoped></style>
