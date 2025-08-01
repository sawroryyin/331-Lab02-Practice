<script setup lang="ts">
import { ref } from 'vue'
import type { Student } from '@/types'
defineProps<{
  student: Student
}>()
</script>

<template>
  <div class="cursor-pointer border border-gray-600 p-[20px] w-[250px] mb-[18px] hover:scale-101 hover:shadow-sp">
      <span>Name:{{ student.name }}</span>
      <br>
      <span>Surname:{{ student.surname }}</span>
      <br>
      <span>GPA:{{ student.gpa }}</span>
  </div>
</template>
