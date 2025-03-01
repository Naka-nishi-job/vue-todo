<script setup lang="ts">
import Todos from "./components/Todos.vue";
</script>

<template>
  <div class="wrapper">
    <h1 class="title">Todo App</h1>
    <Todos />
  </div>
</template>

<style scoped>
.wrapper {
  width: 100%;
  height: 100%;
  margin: 2rem 0;
  padding: 0 2rem;
}

.title {
  color: pink;
}
</style>
