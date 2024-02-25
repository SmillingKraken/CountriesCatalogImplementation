<template>
  <header>
    <ProjectTitle 
      title="Countries Catalog Implementation" 
      @update:search="handleSearch"
      @sort-ascending="sortAscending"
      @sort-descending="sortDescending"
    />
  </header>
  <Countries 
    ref="countries"
    :searchTerm="searchTerm" 
  />
</template>

<script>
  import Countries from './components/Countries.vue';
  import ProjectTitle from './components/ProjectTitle.vue';

  export default {
    components: {
      ProjectTitle,
      Countries
    },
    data () {
      return {
        searchTerm: ''
      }
    },
    methods: {
        handleSearch (term) {
          this.searchTerm =  term
        },
        sortAscending () {
          this.$refs.countries.sortAscending()
        },
        sortDescending() {
          this.$refs.countries.sortDescending()
        }
    }
  }
</script>

