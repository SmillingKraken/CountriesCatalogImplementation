<script setup>
import { ref } from 'vue';

defineProps({
  title: {
    type: String,
    required: true
  }
});

const showCard = ref(false);
const searchTerm = ref('');

const toggleCard = () => {
  showCard.value = !showCard.value;
};
</script>

<template>
  <v-toolbar density="compact">

    <v-toolbar-title>{{ title }}</v-toolbar-title>

    <v-spacer></v-spacer>

    <v-card
      v-if="showCard"
      class="search-card"
      height="100px"
      width="300px"
      img="https://cdn.vuetifyjs.com/images/toolbar/map.jpg"
    >
      <v-text-field
        hide-details
        single-line
        class="transparent-input"
        v-model="searchTerm"
        @input="$emit('update:search', searchTerm)"
      ></v-text-field>
    </v-card>

    <v-btn icon @click="toggleCard">
      <v-icon>mdi-magnify</v-icon>
    </v-btn>

    <v-btn variant="outlined" @click="$emit('sort-ascending')">
      Ascending
    </v-btn>

    <div style="width: 8px;"></div>

    <v-btn variant="outlined" @click="$emit('sort-descending')">
      Descending
    </v-btn>
  </v-toolbar>
</template>

<style>
.search-card {
  background-color: transparent;
  box-shadow: none;
  border: none; 
  margin-top: 50px;
}

/* Keep the existing custom CSS */
.transparent-input .v-text-field__slot input {
  background-color: transparent;
}
</style>

