<script setup lang="ts">
import EventCard from '@/components/EventCard.vue'
import CategoryCard from '@/components/CategoryCard.vue'
import { type Event } from '@/types'
import { ref, onMounted, computed, watchEffect } from 'vue'
import { useRoute, useRouter } from 'vue-router'
import EventService from '@/services/EventService'

const events = ref<Event[] | null>(null)
const totalEvents = ref(0)

const route = useRoute()
const router = useRouter()

// Use query params: page & limit
const page = computed(() => Number(route.query.page) || 1)
const limit = ref(Number(route.query.limit) || 2) // default to 2

function updateLimit(newLimit: number) {
  router.push({ 
    name: 'event-list-view',
    query: { page: 1, limit: newLimit }
  })
}

const hasNexPage = computed(() => {
  const totalPages = Math.ceil(totalEvents.value / limit.value)
  return page.value < totalPages
})

// const props = defineProps({
//   page: {
//     type: Number,
//     required: true
//   }
// })
// const page = computed(() => props.page)

onMounted(() => {
  watchEffect(() => {
    
    EventService.getEvents(limit.value, page.value)
    .then((response) => {
      events.value = response.data
      totalEvents.value = parseInt(response.headers['x-total-count'])
    })
    .catch((error) => {
      console.error('There was an error!', error)
    })
  })
})
</script>

<template>
  <div class="flex flex-col items-center">
    <h1>Events For Good</h1>

    <!-- Dropdown for selecting number of events per page -->
    <label for="limit-select">Events per page:</label>
    <select id="limit-select" v-model.number="limit" @change="updateLimit(limit)" class="border border-gray-300 rounded px-2 py-1 focus:outline-none focus:ring-2 focus:ring-green-500">
      <option :value="1">1</option>
      <option :value="2">2</option>
      <option :value="3">3</option>
      <option :value="4">4</option>
      <option :value="5">5</option>
    </select>

    <!-- new element -->
    <EventCard v-for="event in events" :key="event.id" :event="event" />
    <CategoryCard v-for="event in events" :key="event.id" :event="event" />

    <div class="flex justify-between gap-4 mt-6 w-full max-w-md">
      <div :class="{ 'invisible': page <= 1 }">
        <RouterLink
        :to="{ name: 'event-list-view', query: { page: page - 1, limit: limit}}"
        rel="prev"
        class="bg-gray-200 hover:bg-gray-300 text-gray-800 rounded px-4 py-2 text-left block"
        >&#60; Prev Page</RouterLink>
      </div>

      <div v-if="hasNexPage">
      <RouterLink
        :to="{ name: 'event-list-view', query: { page: page + 1, limit: limit}}"
        rel="next"
        class="bg-gray-200 hover:bg-gray-300 text-gray-800 rounded px-4 py-2 text-right block"
        >Next Page &#62;</RouterLink>
      </div>
    </div>
  </div>
</template>