<script setup lang="ts">
import { useEventStore } from '@/stores/event'
import { storeToRefs } from 'pinia'

const store = useEventStore()
const { event } = storeToRefs(store)

</script>

<template>
    <div v-if="event" class="p-6">
        <h1 class="text-3xl font-bold mb-4">{{ event.title }}</h1>
        <nav class="pb-6 space-x-4">
            <router-link :to="{ name: 'event-detail-view'}" class="font-bold text-gray-800 hover:text-green-500">Details</router-link>
            <span class="text-gray-400">|</span>
            <router-link :to="{ name: 'event-register-view'}" class="font-bold text-gray-800 hover:text-green-500">Register</router-link>
            <span class="text-gray-400">|</span>
            <router-link :to="{ name: 'event-edit-view'}" class="font-bold text-gray-800 hover:text-green-500">Edit</router-link>
        </nav>
        <RouterView :event="event"/>
    </div>
</template>