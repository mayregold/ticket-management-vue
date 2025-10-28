<template>
  <div class="border p-3 rounded flex flex-col md:flex-row md:items-start md:justify-between gap-3">
    <div class="flex-1">
      <!-- VIEW MODE -->
      <template v-if="!editing">
        <div class="flex items-center gap-4">
          <div class="font-semibold">{{ ticket.title }}</div>
          <div class="text-sm text-slate-500">[{{ ticket.priority }}]</div>
          <div class="ml-auto text-sm">{{ ticket.status }}</div>
        </div>
        <p class="mt-2 text-sm text-slate-600">{{ ticket.description }}</p>
        <p v-if="ticket.resolution" class="mt-2 text-sm text-green-700">
          Resolution: {{ ticket.resolution }}
        </p>
      </template>

    
      <template v-else>
        <input
          name="title"
          v-model="form.title"
          class="w-full input mb-2"
        />
        <textarea
          name="description"
          v-model="form.description"
          class="w-full input h-20 mb-2"
        ></textarea>

        <div class="flex gap-2">
          <select name="priority" v-model="form.priority" class="input">
            <option value="low">Basic</option>
            <option value="medium">Standard</option>
            <option value="high">Critical</option>
          </select>

          <select name="status" v-model="form.status" class="input">
            <option value="open">Open</option>
            <option value="pending">Pending</option>
            <option value="closed">Closed</option>
          </select>
        </div>

        <div v-if="errors.resolution" class="text-red-600 text-sm mt-1">
          {{ errors.resolution }}
        </div>
      </template>
    </div>

    <div class="flex flex-col gap-2">
      <!-- BUTTONS -->
      <template v-if="!editing">
        <button @click="editing = true" class="px-3 py-1 border rounded">Edit</button>
        <button @click="removeTicket" class="px-3 py-1 border rounded">Delete</button>
      </template>
      <template v-else>
        <button @click="save" class="px-3 py-1 bg-teal-500 text-white rounded">Save</button>
        <button @click="cancel" class="px-3 py-1 border rounded">Cancel</button>
      </template>
    </div>
  </div>
</template>

<script setup>
import { reactive, ref } from 'vue'
import ticketService from '../services/ticketService'
import { validateTicket } from '../utils/validators'

const props = defineProps({
  ticket: Object
})
const emit = defineEmits(['change'])

const editing = ref(false)
const form = reactive({ ...props.ticket })
const errors = reactive({})

function save() {
  // Validation for resolution when closing
  if (form.status === 'closed' && (!form.resolution || form.resolution.trim().length < 3)) {
    errors.resolution = 'Provide a resolution before closing'
    return
  }

  Object.assign(errors, validateTicket(form))
  if (Object.keys(errors).length) return

  try {
    ticketService.update(props.ticket.id, form)
    editing.value = false
    emit('change')
  } catch {
    errors.server = 'Failed to save'
  }
}

function cancel() {
  Object.assign(form, props.ticket)
  Object.keys(errors).forEach(k => delete errors[k])
  editing.value = false
}

function removeTicket() {
  if (!confirm('Delete this ticket?')) return
  try {
    ticketService.remove(props.ticket.id)
    emit('change')
  } catch {
    alert('Failed to delete')
  }
}
</script>
