<script setup>
import { RouterView } from 'vue-router'
import Navbar from './components/Navbar.vue'  // 👈 import your Navbar component
</script>

<template>
  <div class="min-h-screen bg-slate-50 text-slate-800 flex flex-col">
    <!-- 👇 Navbar always visible -->
    <Navbar />  

    <!-- 👇 Page content changes based on the route -->
    <main class="flex-1">
      <RouterView />
    </main>
  </div>
</template>

<style>
body {
  margin: 0;
  font-family: 'Inter', sans-serif;
}
</style>
