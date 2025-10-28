<template>
  <div>
    <div v-if="!tickets || !tickets.length" class="bg-white p-6 rounded shadow text-center">
      No tickets yet
    </div>
    <div v-else class="bg-white p-4 rounded shadow">
      <div class="grid gap-2">
        <TicketItem
          v-for="t in tickets"
          :key="t.id"
          :ticket="t"
          @change="onChange"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
import TicketItem from './TicketItem.vue'

defineProps({
  tickets: {
    type: Array,
    default: () => []
  },
  onChange: Function
})
</script>
