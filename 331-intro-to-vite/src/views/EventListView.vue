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
  <div class="events">
    <h1>Events For Good</h1>

    <!-- Dropdown for selecting number of events per page -->
    <label for="limit-select">Events per page:</label>
    <select id="limit-select" v-model.number="limit" @change="updateLimit(limit)">
      <option :value="1">1</option>
      <option :value="2">2</option>
      <option :value="3">3</option>
      <option :value="4">4</option>
      <option :value="5">5</option>
    </select>

    <!-- new element -->
    <EventCard v-for="event in events" :key="event.id" :event="event" />
    <CategoryCard v-for="event in events" :key="event.id" :event="event" />

    <div class="pagination">
      <div id="page-prev-wrapper" v-if="page > 1">
        <RouterLink
        id="page-prev"
        :to="{ name: 'event-list-view', query: { page: page - 1, limit: limit}}"
        rel="prev"
        v-if="page > 1"
        >&#60; Prev Page</RouterLink>
      </div>

      <div id="page-next-wrapper" v-if="hasNexPage">
      <RouterLink
        id="page-next"
        :to="{ name: 'event-list-view', query: { page: page + 1, limit: limit}}"
        rel="next"
        v-if="hasNexPage"
        >Next Page &#62;</RouterLink>
      </div>
    </div>
  </div>
</template>

<style scoped>
.events {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.pagination {
  display: flex;
  gap: 1rem;
  margin-top: 1em;
  justify-content: space-between;
  width: auto;
}
.pagination a {
  text-decoration: none;
  color: #2c3e50;
  padding: 0.5rem 1rem;
  border-radius: 5px;
  background-color: #eee;
  transition: background-color 0.2s;
}
#page-prev {
  text-align: left;
}
#page-next {
  text-align: right;
}
select {
  margin-bottom: 1rem;
}
.pagination a:hover {
  background-color: #ccc;
}
#page-prev-wrapper,
#page-next-wrapper {
  flex-shrink: 0;
}
</style>