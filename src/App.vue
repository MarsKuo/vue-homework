<script setup>
import { RouterLink, RouterView } from 'vue-router'

</script>

<template>
  <div class="conatiner">
    <nav>
      <RouterLink to="/">Home</RouterLink> |
      <RouterLink to="/week1">Week1</RouterLink> |
      <RouterLink to="/week2">Week2</RouterLink> |
      <RouterLink to="/week3">Week3</RouterLink> |
    </nav>
  </div>


  <RouterView />
</template>

<style scoped></style>
