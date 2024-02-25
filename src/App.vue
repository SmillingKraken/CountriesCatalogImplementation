<script setup>
  import { ref } from 'vue';
  import Countries from './components/Countries.vue';
  import ProjectTitle from './components/ProjectTitle.vue';

  const countriesComponent = ref(null);
  const searchTerm = ref('');

  const handleSearch = (term) => {
    searchTerm.value = term;
  };

  const sortAscending = () => {
    if (countriesComponent.value) {
      console.log(countriesComponent.value)
      countriesComponent.value.sortAscending();
    } 
  };

  const sortDescending = () => {
    if (countriesComponent.value) {
      countriesComponent.value.sortDescending();
    }
  };
</script>

<template>
  <header>
    <ProjectTitle 
      title="Countries Catalog Implementation" 
      @update:search="handleSearch"
      @sort-ascending="sortAscending"
      @sort-descending="sortDescending"
    />
  </header>

  <main>
    <Countries 
      ref="countriesComponent"
      :searchTerm="searchTerm" 
    />
  </main>
</template>
