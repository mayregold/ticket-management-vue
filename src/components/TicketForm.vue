<template>
  <form @submit.prevent="submit" class="bg-white rounded p-6 shadow">
    <ErrorBanner v-if="serverError" :message="serverError" />
    <h3 class="text-lg font-semibold mb-3">Create ticket</h3>

    <div class="grid grid-cols-1 md:grid-cols-2 gap-3">
      <div>
        <label>
          Title
          <input
            name="title"
            v-model="form.title"
            class="w-full mt-1 input"
          />
        </label>
        <div v-if="errors.title" class="text-red-600 text-sm">{{ errors.title }}</div>
      </div>

      <div>
        <label>
          Priority
          <select
            name="priority"
            v-model="form.priority"
            class="w-full mt-1 input"
          >
            <option value="low">Basic</option>
            <option value="medium">Standard</option>
            <option value="high">Critical</option>
          </select>
        </label>
        <div v-if="errors.priority" class="text-red-600 text-sm">{{ errors.priority }}</div>
      </div>

      <div class="md:col-span-2">
        <label>
          Description
          <textarea
            name="description"
            v-model="form.description"
            class="w-full mt-1 input h-24"
          ></textarea>
        </label>
        <div v-if="errors.description" class="text-red-600 text-sm">{{ errors.description }}</div>
      </div>
    </div>

    <div class="mt-4 flex gap-3">
      <button type="submit" class="px-4 py-2 bg-teal-500 text-white rounded">Create</button>
    </div>
  </form>
</template>

<script setup>
import { reactive, ref } from 'vue'
import ErrorBanner from './ErrorBanner.vue'
import ticketService from '../services/ticketService'
import { validateTicket } from '../utils/validators'

const form = reactive({
  title: '',
  description: '',
  priority: 'medium',
  status: 'open',
  resolution: ''
})

const errors = reactive({})
const serverError = ref(null)

const emit = defineEmits(['create'])

function submit() {
  Object.assign(errors, validateTicket(form))
  if (Object.keys(errors).length) return

  try {
    ticketService.create({ ...form })
    Object.assign(form, {
      title: '',
      description: '',
      priority: 'medium',
      status: 'open',
      resolution: ''
    })
    serverError.value = null
    emit('create')
  } catch (err) {
    serverError.value = 'Could not create ticket'
  }
}
</script>
