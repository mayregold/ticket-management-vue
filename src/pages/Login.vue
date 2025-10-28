<template>
  <div class="min-h-screen flex items-center justify-center bg-gradient-to-br from-slate-50 via-white to-teal-50 px-4">
    <div class="w-full max-w-md bg-white/90 backdrop-blur-md rounded-2xl shadow-lg p-8 border border-slate-100">
      <div class="text-center mb-6">
        <div class="mx-auto w-16 h-16 bg-teal-100 text-teal-600 flex items-center justify-center rounded-full text-3xl font-bold shadow-sm">
          🎫
        </div>
        <h2 class="text-2xl font-bold text-slate-800 mt-4">Welcome Back</h2>
        <p class="text-sm text-slate-500 mt-1">Sign in to your account to continue</p>
      </div>

      <ErrorBanner v-if="serverError" :message="serverError" />

      <form @submit.prevent="onSubmit" class="space-y-5">
        <div>
          <label class="block text-sm font-medium text-slate-600 mb-1">Username</label>
          <input
            v-model="form.username"
            name="username"
            placeholder="Enter your username"
            class="w-full px-4 py-2.5 border border-slate-200 rounded-lg focus:ring-2 focus:ring-teal-400 focus:outline-none transition"
          />
        </div>

        <div>
          <label class="block text-sm font-medium text-slate-600 mb-1">Password</label>
          <input
            type="password"
            v-model="form.password"
            name="password"
            placeholder="••••••••"
            class="w-full px-4 py-2.5 border border-slate-200 rounded-lg focus:ring-2 focus:ring-teal-400 focus:outline-none transition"
          />
        </div>

        <button
          type="submit"
          class="w-full py-2.5 mt-2 bg-teal-600 hover:bg-teal-700 text-white font-semibold rounded-lg shadow-sm transition duration-200"
        >
          Sign In
        </button>

        <div class="text-center text-sm text-slate-500 mt-4">
          Don’t have an account?
          <RouterLink to="/register" class="text-teal-600 font-medium hover:underline">
            Register
          </RouterLink>
        </div>
      </form>
    </div>
  </div>
</template>

<script setup>
import { reactive, ref } from "vue"
import { useRouter } from "vue-router"
import ErrorBanner from "../components/ErrorBanner.vue"
import authService from "../services/authService"

const form = reactive({ username: "", password: "" })
const serverError = ref(null)
const router = useRouter()

function onSubmit() {
  try {
    authService.login(form)
    router.push("/dashboard")
  } catch (err) {
    serverError.value = err.message || "Login failed"
  }
}
</script>
