<script setup>
import Countries from './components/Countries.vue';
import ProjectTittle from './components/ProjectTittle.vue';
</script>

<template>
  <header>
    <div class="wrapper">
      <ProjectTittle msg="Countries Catalog Implementation" />
    </div>
  </header>

  <main>
    <Countries/>
  </main>
</template>

<style scoped>

  .wrapper {
      display: flex;
      justify-content: center; 
      align-items: center;     
    }

</style>
