<script setup>
import { RouterLink } from 'vue-router'
</script>

<template>
  <header class="bg-[#f1f1f1]">
    <nav class="container mx-auto flex items-center px-6 py-4">
      <div class="flex items-center gap-2">
        <img class="w-12" src="../assets/Vue_Logo_Black.png" alt="" />
        <h1 class="text-xl font-medium">Vue Todos</h1>
      </div>
      <ul class="flex flex-1 justify-end gap-3">
        <RouterLink to="/">Home</RouterLink>
        <RouterLink to="/about">About</RouterLink>
      </ul>
    </nav>
  </header>
</template>
