<script setup lang="ts">
import EventCard from '@/components/EventCard.vue'
import CategoryCard from '@/components/CategoryCard.vue'
import type { Event } from '@/types'
import { ref, onMounted } from 'vue'
import EventService from '@/services/EventService'

const events = ref<Event[]>()

onMounted(() => {
  EventService.getEvents()
  .then((response) => {
    events.value = response.data
  })
  .catch((error) => {
    console.error('There was an error', error)
  })
})
</script>

<template>
  <div class="events">
    <h1>Events For Good</h1>
    <!-- new element -->
    <EventCard v-for="event in events" :key="event.id" :event="event" />
    <CategoryCard v-for="event in events" :key="event.id" :event="event" />
  </div>
</template>

<style scoped>
.events {
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>