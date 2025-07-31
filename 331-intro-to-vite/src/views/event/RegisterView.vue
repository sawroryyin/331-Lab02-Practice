<script setup lang="ts">
import { toRefs } from 'vue'
import { type Event} from '@/types'
import { useRouter } from 'vue-router'
import { useMessageStore } from '@/stores/message';

const props = defineProps<{
    event: Event,
    id: String
}> ()

// eslint-disable-next-line @typescript-eslint/no-unused-vars

const { event } = toRefs(props)
const router = useRouter()
const store = useMessageStore()
const register = () => {
    // If the registration API call successful
    // Push back to the event details view
    // router.push({ name: 'event-detail-view'})
    store.updateMessage('You are successfully registered for ' + props.event.title)
    setTimeout(() => {
        store.resetMessage()
    }, 3000)
    router.push({ name: 'event-detail-view', params: { id: props.event.id} })
}
</script>
<template>
    <p>Register event here</p>
    <button class="bg-gray-300 border border-gray-600 px-[10px] py-[5px] rounded-[3px] text-black font-sans cursor-pointer text-sm hover:bg-[#e0e0e0] active:bg-[#c0c0c0] active:border-[#888]" @click="register">Register</button>
</template>