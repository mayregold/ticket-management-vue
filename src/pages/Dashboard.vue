<template>
  <div class="max-w-6xl mx-auto p-6 space-y-8">
    <div class="bg-white p-8 rounded-2xl shadow-lg">
      <h3 class="text-2xl font-bold text-gray-800 mb-6">📊 Ticket Analytics</h3>
      <div class="grid gap-6 sm:grid-cols-2 md:grid-cols-3">
        <div
          v-for="(s, i) in stats"
          :key="i"
          class="rounded-xl bg-gradient-to-br p-5 text-white shadow-lg hover:scale-105 transform transition"
          :class="s.color"
        >
          <div class="flex items-center justify-between">
            <div>
              <p class="text-sm opacity-90">{{ s.title }}</p>
              <p class="text-3xl font-bold mt-1">{{ s.count }}</p>
            </div>
            <div class="bg-white/20 p-3 rounded-full">
              <component :is="s.icon" class="h-8 w-8 text-white" />
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="bg-white p-8 rounded-2xl shadow-lg">
      <h3 class="text-2xl font-bold text-gray-800 mb-4">🎟️ Recent Tickets</h3>

      <p v-if="tickets.length === 0" class="text-gray-500 text-sm">
        No tickets yet — create your first one!
      </p>

      <ul v-else class="divide-y divide-gray-100">
        <li
          v-for="t in tickets.slice(0, 5)"
          :key="t.id"
          class="py-4 flex items-center justify-between hover:bg-slate-50 transition rounded px-2"
        >
          <div>
            <p class="font-semibold text-gray-800">{{ t.title }}</p>
            <p class="text-sm text-gray-500">{{ t.description.slice(0, 60) }}...</p>
          </div>
          <span
            class="px-3 py-1 rounded-full text-xs font-medium"
            :class="{
              'bg-rose-100 text-rose-600': t.status === 'open',
              'bg-amber-100 text-amber-600': t.status === 'pending',
              'bg-emerald-100 text-emerald-600': t.status === 'closed'
            }"
          >
            {{ t.status }}
          </span>
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup>
import { ChartBarIcon, ClockIcon, CheckCircleIcon } from '@heroicons/vue/24/solid'
import ticketService from '../services/ticketService'

const tickets = ticketService.list()

const counts = tickets.reduce((acc, t) => {
  acc[t.status] = (acc[t.status] || 0) + 1
  return acc
}, {})

const stats = [
  { title: 'Open Tickets', count: counts.open || 0, color: 'from-rose-400 to-pink-500', icon: ChartBarIcon },
  { title: 'Pending Tickets', count: counts.pending || 0, color: 'from-amber-400 to-orange-500', icon: ClockIcon },
  { title: 'Closed Tickets', count: counts.closed || 0, color: 'from-emerald-400 to-teal-500', icon: CheckCircleIcon }
]
</script>
