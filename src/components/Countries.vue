<script setup>
  import { defineProps } from "vue";
  import Country from "./Country.vue";
  defineProps({
    searchTerm: String,
  });
</script>

<template>
  <div class="container m-5">
    <v-row justify="center" class="py-5">
      <v-col
        v-for="(country, index) in filteredCountries"
        :key="index"
        cols="12"
        sm="6"
        md="4"
      >
        <Country
          :flagUrl="country.flag"
          :nativeNames="country.nativeNames"
          :countryName="country.name"
          :cca2="country.cca2"
          :cca3="country.cca3"
          :altSpellings="country.altSpellings"
          :idd="country.idd ?? 'no idd'"
        >
        </Country>
      </v-col>
    </v-row>
  </div>
  <div class="pagination">
    <v-btn variant="outlined" @click="prevPage" :disabled="currentPage <= 1">
      Previous
    </v-btn>

    <span>Page {{ currentPage }}</span>

    <v-btn variant="outlined" @click="nextPage" :disabled="currentPage >= totalPages">
      Next
    </v-btn>
  </div>
</template>

<style>
.pagination {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
}
</style>

<script>
import axios from "axios";

export default {
  data() {
    return {
      filteredData: null,
      currentPage: 1,
      rowsPerPage: 25,
    };
  },
  methods: {
    sortAscending() {
      if (!this.filteredData) return;
      this.filteredData.sort((a, b) => a.name.localeCompare(b.name));
    },
    sortDescending() {
      if (!this.filteredData) return;
      this.filteredData.sort((a, b) => b.name.localeCompare(a.name));
    },
    nextPage() {
      if (this.currentPage * this.rowsPerPage < this.totalFilteredCountries) {
        this.currentPage++;
      }
    },
    prevPage() {
      if (this.currentPage > 1) this.currentPage--;
    },
  },
  computed: {
    // Computed property to filter the country list based on the search term
    filteredCountries() {
      let data = this.filteredData;
      if (!data) return [];

      // Filter based on search term
      if (this.searchTerm) {
        data = data.filter((country) =>
          country.name.toLowerCase().includes(this.searchTerm.toLowerCase())
        );
      }

      const start = (this.currentPage - 1) * this.rowsPerPage;
      const end = start + this.rowsPerPage;
      return data.slice(start, end);
    },
    totalFilteredCountries() {
      let data = this.filteredData;
      if (!data) return 0;

      if (this.searchTerm) {
        data = data.filter((country) =>
          country.name.toLowerCase().includes(this.searchTerm.toLowerCase())
        );
      }

      return data.length;
    },
    totalPages() {
      return Math.ceil(this.filteredCountriesLength / this.rowsPerPage);
    },
  },
  mounted() {
    axios
      .get("https://restcountries.com/v3.1/all")
      .then((response) => {
        const countries = response.data.map((country) => {
          return {
            flag: country.flags.png,
            name: country.name.official,
            cca2: country.cca2,
            cca3: country.cca3,
            nativeNames: country.name.nativeName
              ? Object.keys(country.name.nativeName).map((key) => {
                  return {
                    languageCode: key,
                    official: country.name.nativeName[key].official,
                    common: country.name.nativeName[key].common,
                  };
                })
              : "this country have no native name",
            altSpellings: country.altSpellings
              ? Object.keys(country.altSpellings).map((key) => {
                  return { altName: country.altSpellings[key] };
                })
              : "no eng altSpelling",
              idd: country.idd?.suffixes?.[0] ?? "no idd",
          };
        });
        this.filteredData = countries;
      })
      .catch((error) => console.error("Error:", error));
  },
};
</script>
