<script setup lang="ts">
import { ref, onMounted } from 'vue'
import { RouterLink, RouterView } from 'vue-router'
import { useMessageStore } from './stores/message'
import { storeToRefs } from 'pinia'
import { SpeedInsights } from "@vercel/speed-insights/vue"
const store = useMessageStore()
const { message } = storeToRefs(store)


const events = ref<Event[]>()
</script>

<template>
  <SpeedInsights/>
  <div class="text-center font-sans text-gray-700 antialias">
    <header>
      <div class="animate-fade" v-if="message.includes('register')">
        <h4>{{ message }}</h4>
      </div>
      <h1>Deploy with Vercel</h1>
      <div>
        <nav class="py-6"> 
          <RouterLink class="font-bold text-gray-700" exact-active-class="text-green-500" :to="{ name: 'event-list-view'}">Event</RouterLink> |
          <RouterLink class="font-bold text-gray-700" exact-active-class="text-green-500" :to="{ name: 'about'}">About</RouterLink> |
          <RouterLink class="font-bold text-gray-700" exact-active-class="text-green-500" :to="{ name: 'info'}">Info</RouterLink>
        </nav>
      </div>
    </header>
    <RouterView/>
  </div>
</template>


