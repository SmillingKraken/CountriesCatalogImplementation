<script setup>
import Country from './Country.vue';
</script>

<template>
  <div class="container m-5 ">
    <div class="row justify-content-center">
      <div v-if="filteredData">
        <div v-for="(country, index) in filteredData" :key="index">
          <Country :flagUrl="country.flag"
          :nativeNames="country.nativeNames">
            <h2>Country Name: {{ country.name }}</h2>
            <h2>2 Character Country Code: {{ country.cca2 }}</h2>
            <h2>3 Character Country Code:: {{ country.cca3 }}</h2>
            <div v-if="nativeNames">
              <div v-for="(nName, index) in nativeNames" :key="index"> 
                <h2>Native Name: {{ nName.languageCode }}</h2>
                <h2>Native Name Official: {{ nName.official }}</h2>
                <h2>Native Name Common: {{ nName.common }}</h2>
              </div>
            </div>
            <h2>Alternative Country Name: {{ country.name }}</h2>
            <h2>Country Calling Code: {{ country.name }}</h2>
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
            flag: country.flags.png,
            name: country.name.official,
            cca2: country.cca2,
            cca3: country.cca3,
            nativeNames: country.name.nativeName ? Object.keys(country.name.nativeName).map(key => {
              return {
                languageCode: key,
                official: country.name.nativeName[key].official,
                common: country.name.nativeName[key].common
              };
            }) : "this country have no native name",
            altSpellings: country.altSpellings,
            idd: country.idd
          };
        });
        this.filteredData = countries;
        console.log(countries); 
      })
      .catch(error => console.error('Error:', error));
  }
};
</script>