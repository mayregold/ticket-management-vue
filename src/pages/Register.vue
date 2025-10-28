<template>
  <div class="min-h-screen flex items-center justify-center bg-gradient-to-br from-slate-50 via-white to-teal-50 px-4">
    <div class="w-full max-w-md bg-white/90 backdrop-blur-md rounded-2xl shadow-lg p-8 border border-slate-100">
      <div class="text-center mb-6">
        <div class="mx-auto w-16 h-16 bg-teal-100 text-teal-600 flex items-center justify-center rounded-full text-3xl font-bold shadow-sm">
          🧾
        </div>
        <h2 class="text-2xl font-bold text-slate-800 mt-4">Create Account</h2>
        <p class="text-sm text-slate-500 mt-1">
          Join us today to manage your tickets efficiently
        </p>
      </div>

      <ErrorBanner v-if="serverError" :message="serverError" />

      <form @submit.prevent="onSubmit" class="space-y-5">
        <!-- Username -->
        <div>
          <label class="block text-sm font-medium text-slate-600 mb-1">
            Username
          </label>
          <input
            v-model="form.username"
            name="username"
            placeholder="Choose a username"
            class="w-full px-4 py-2.5 border rounded-lg focus:ring-2 focus:ring-teal-400 focus:outline-none transition"
            :class="errors.username ? 'border-red-400' : 'border-slate-200'"
          />
          <div v-if="errors.username" class="text-red-600 text-sm mt-1">
            {{ errors.username }}
          </div>
        </div>

        <!-- Password -->
        <div>
          <label class="block text-sm font-medium text-slate-600 mb-1">
            Password
          </label>
          <input
            type="password"
            v-model="form.password"
            name="password"
            placeholder="Create a secure password"
            class="w-full px-4 py-2.5 border rounded-lg focus:ring-2 focus:ring-teal-400 focus:outline-none transition"
            :class="errors.password ? 'border-red-400' : 'border-slate-200'"
          />
          <div v-if="errors.password" class="text-red-600 text-sm mt-1">
            {{ errors.password }}
          </div>
        </div>

        <!-- Submit -->
        <button
          type="submit"
          class="w-full py-2.5 mt-2 bg-teal-600 hover:bg-teal-700 text-white font-semibold rounded-lg shadow-sm transition duration-200"
        >
          Register
        </button>

        <div class="text-center text-sm text-slate-500 mt-4">
          Already have an account?
          <RouterLink to="/login" class="text-teal-600 font-medium hover:underline">
            Login
          </RouterLink>
        </div>
      </form>
    </div>
  </div>
</template>

<script setup>
import { reactive, ref } from 'vue'
import { useRouter } from 'vue-router'
import authService from '../services/authService'
import { validateRegister } from '../utils/validators'
import ErrorBanner from '../components/ErrorBanner.vue'

const router = useRouter()

const form = reactive({ username: '', password: '' })
const errors = reactive({})
const serverError = ref(null)

function onSubmit() {
  const v = validateRegister(form)
  Object.assign(errors, v)
  if (Object.keys(v).length) return

  try {
    authService.register(form)
    router.push('/dashboard')
  } catch (err) {
    serverError.value = err.message
  }
}
</script>
