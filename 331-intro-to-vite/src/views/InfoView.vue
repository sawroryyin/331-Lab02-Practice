<script setup lang="ts">
import InfoCard from '@/components/InfoCard.vue'
import type { Student } from '@/types'
import { ref, onMounted } from 'vue'
import InfoService from '@/services/InfoService'

const students = ref<Student[]>([])

onMounted(() => {
  InfoService.getInfos()
  .then((response) => {
    students.value = response.data
  })
  .catch((error) => {
    console.error('There was an error', error)
  })
})
</script>

<template>
  <div class="flex flex-col items-center">
    <h1>Events For Good</h1>
    <!-- new element -->
    <!-- <EventCard v-for="student in students" :key="student.id" :name="event" />
    <CategoryCard v-for="student in students" :key="student.id" :event="event" /> -->
    <InfoCard v-for="student in students" :key="student.id" :student="student" />
  </div>
</template>