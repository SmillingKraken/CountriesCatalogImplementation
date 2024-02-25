<script setup>
import { ref } from 'vue';
import Countries from './components/Countries.vue';
import ProjectTitle from './components/ProjectTitle.vue';

const searchTerm = ref('');

const handleSearch = (term) => {
  searchTerm.value = term;
};
</script>

<template>
  <header>
    <ProjectTitle 
      title="Countries Catalog Implementation" 
      @update:search="handleSearch"
    />
  </header>

  <main>
    <Countries 
      ref="countriesComponent"
      :searchTerm="searchTerm" 
    />
  </main>
</template>

<script>
