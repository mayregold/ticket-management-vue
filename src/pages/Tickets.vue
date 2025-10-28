<template>
  <div class="max-w-screen-lg mx-auto grid gap-6">
    <ErrorBanner v-if="error" :message="error" />
    <TicketForm @create="refresh" />
    <TicketList :tickets="tickets" @change="refresh" />
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import ticketService from '../services/ticketService'
import TicketList from '../components/TicketList.vue'
import TicketForm from '../components/TicketForm.vue'
import ErrorBanner from '../components/ErrorBanner.vue'

const tickets = ref([])
const error = ref(null)

function refresh() {
  try {
    tickets.value = ticketService.list()
  } catch (err) {
    error.value = 'Failed to load tickets'
  }
}

onMounted(() => {
  refresh()
})
</script>
