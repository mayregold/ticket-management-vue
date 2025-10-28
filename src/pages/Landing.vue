<template>
  <section class="relative bg-gradient-to-br from-teal-50 via-white to-cyan-50 overflow-hidden">
    <div class="absolute inset-0 opacity-60 pointer-events-none">
      <Waves />
    </div>

    <div
      class="relative max-w-screen-xl mx-auto px-6 py-16 md:py-24 flex flex-col-reverse md:flex-row items-center gap-10"
    >
      <div class="flex-1 text-center md:text-left">
        <h1 class="text-4xl sm:text-5xl md:text-6xl font-extrabold text-slate-800 leading-tight">
          Manage Your <span class="text-teal-600">Tickets</span> with Ease
        </h1>
        <p class="mt-4 text-base sm:text-lg text-slate-600 max-w-lg mx-auto md:mx-0">
          Ticketly helps you track issues, organize support requests, and deliver faster
          solutions. Perfect for teams who value efficiency and clarity.
        </p>

        <div
          class="mt-8 flex flex-col sm:flex-row gap-4 justify-center md:justify-start"
        >
          <router-link
            to="/register"
            class="px-6 py-3 bg-teal-600 hover:bg-teal-700 text-white text-lg font-medium rounded-lg shadow-md transition-transform hover:-translate-y-1 hover:shadow-lg"
          >
            🎟️ Get Started
          </router-link>
          <router-link
            to="/dashboard"
            class="px-6 py-3 border border-teal-600 text-teal-700 hover:bg-teal-50 text-lg font-medium rounded-lg transition-colors"
          >
            View Dashboard
          </router-link>
        </div>
      </div>

      <div class="flex-1 flex justify-center">
        <div
          class="bg-gradient-to-tr from-teal-500 to-cyan-600 rounded-3xl p-10 shadow-2xl transform hover:scale-105 transition-transform duration-300 w-64 sm:w-72 md:w-80 text-center"
        >
          <div class="text-7xl mb-4 animate-pulse">🎫</div>
          <p class="text-white font-semibold text-xl sm:text-2xl">Book • Track • Manage</p>
          <p class="text-cyan-100 text-sm mt-2">Your all-in-one ticket solution</p>
        </div>
      </div>
    </div>

    <div class="h-16 bg-gradient-to-t from-white to-transparent"></div>
  </section>
</template>

<script>
import Waves from '../components/Waves.vue'

export default {
  name: 'Landing',
  components: {
    Waves
  }
}
</script>
