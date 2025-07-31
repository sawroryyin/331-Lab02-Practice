<script setup lang="ts">
import { toRefs } from 'vue'
import { type Event} from '@/types'
import { useMessageStore } from '@/stores/message'
import { useRouter } from 'vue-router';

const props = defineProps<{
    event: Event,
    id: String
}> ()

const router = useRouter()
const store = useMessageStore()
const edit = () => {
    store.updateMessage('Your data has been updated for ' + props.event.title)
    setTimeout(() => {
        store.resetMessage()
    }, 3000)
    router.push({ name: 'event-detail-view', params: { id: props.event.id } })
}

// eslint-disable-next-line @typescrpit-eslint/no-unused-vars

const { event } = toRefs(props)
</script>

<template>
    <p>Edit event here</p>
    <button class="bg-gray-300 border border-gray-600 px-[10px] py-[5px] rounded-[3px] text-black font-sans cursor-pointer text-sm hover:bg-[#e0e0e0] active:bg-[#c0c0c0] active:border-[#888]" @click="edit">Edit</button>
</template>