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
    <button @click="edit">Edit</button>
</template>