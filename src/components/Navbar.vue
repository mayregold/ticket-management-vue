<template>
  <header class="bg-white shadow-sm fixed w-full top-0 left-0 z-50">
    <div class="max-w-screen-xl mx-auto flex items-center justify-between p-4">
      <!-- Left side -->
      <div class="flex items-center gap-4">
        <RouterLink to="/" class="text-2xl font-bold text-teal-600">Ticketly</RouterLink>

        <!-- Desktop Nav -->
        <nav class="hidden md:flex gap-4">
          <RouterLink to="/" class="px-3 py-2 rounded hover:bg-slate-100">Home</RouterLink>
          <RouterLink
            v-if="user"
            to="/dashboard"
            class="px-3 py-2 rounded hover:bg-slate-100"
          >
            Dashboard
          </RouterLink>
          <RouterLink
            v-if="user"
            to="/tickets"
            class="px-3 py-2 rounded hover:bg-slate-100"
          >
            Tickets
          </RouterLink>
        </nav>
      </div>

      <!-- Right side (Desktop) -->
      <div class="hidden md:flex items-center">
        <template v-if="!user">
          <RouterLink to="/login" class="px-3 py-2 mr-2 hover:text-teal-600">Login</RouterLink>
          <RouterLink
            to="/register"
            class="px-3 py-2 rounded bg-teal-600 text-white hover:bg-teal-700"
          >
            Register
          </RouterLink>
        </template>

        <template v-else>
          <div class="flex items-center gap-3">
            <span class="text-sm text-slate-600">Hi, {{ user.username }}</span>
            <button
              @click="logout"
              class="px-3 py-2 rounded bg-red-100 hover:bg-red-200 text-red-600"
            >
              Logout
            </button>
          </div>
        </template>
      </div>

      <!-- Mobile Menu Button -->
      <button
        class="text-2xl text-teal-600 md:hidden focus:outline-none"
        @click="isOpen = !isOpen"
      >
        <i v-if="!isOpen" class="fas fa-bars"></i>
        <i v-else class="fas fa-times"></i>
      </button>
    </div>

    <!-- Mobile Dropdown Menu -->
    <div
      v-if="isOpen"
      class="md:hidden bg-white border-t border-slate-200 py-3 px-4 space-y-2 shadow-sm"
    >
      <RouterLink to="/" class="block py-2 hover:text-teal-600" @click="closeMenu">Home</RouterLink>

      <RouterLink
        v-if="user"
        to="/dashboard"
        class="block py-2 hover:text-teal-600"
        @click="closeMenu"
      >
        Dashboard
      </RouterLink>

      <RouterLink
        v-if="user"
        to="/tickets"
        class="block py-2 hover:text-teal-600"
        @click="closeMenu"
      >
        Tickets
      </RouterLink>

      <template v-if="!user">
        <RouterLink to="/login" class="block py-2 hover:text-teal-600" @click="closeMenu">
          Login
        </RouterLink>
        <RouterLink
          to="/register"
          class="block py-2 text-white bg-teal-600 rounded text-center hover:bg-teal-700"
          @click="closeMenu"
        >
          Register
        </RouterLink>
      </template>

      <template v-else>
        <button
          @click="logout"
          class="w-full py-2 rounded bg-red-100 text-red-600 hover:bg-red-200"
        >
          Logout
        </button>
      </template>
    </div>
  </header>
</template>

<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import authService from '../services/authService'

const router = useRouter()
const user = authService.getCurrentUser()
const isOpen = ref(false)

function logout() {
  authService.logout()
  router.push('/login')
  isOpen.value = false
}

function closeMenu() {
  isOpen.value = false
}
</script>

<style>
body {
  padding-top: 70px; /* prevent content overlap with fixed navbar */
}
</style>
