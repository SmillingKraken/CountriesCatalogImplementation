<script setup>
import Country from './Country.vue';
</script>

<template>
  <div class="container m-5 ">
    <div class="row justify-content-center">
      <div v-if="filteredData">
        <div v-for="(country, index) in filteredData" :key="index">
          <Country>
            <template #icon/>
            <h2>{{ country.name }}</h2>
          </Country>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      filteredData: null,
    };
  },
  mounted() {
    axios.get('https://restcountries.com/v3.1/all')
      .then(response => {
        const countries = response.data.map(country => {
          return {
            name: country.name.official,
           
          };
        });
        this.filteredData = countries;
        console.log(countries); 
      })
      .catch(error => console.error('Error:', error));
  }
};
</script>