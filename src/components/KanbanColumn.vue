<template>
  <div
    class="kanban-column bg-white shadow rounded flex-1 mx-2"
    @dragover.prevent
    @drop="drop($event)"
  >
    <div v-if="status === 'New'" class="bg-purple-500 py-9 px-16 w-full">
      <h2 class="w-20 font-bold text-black">{{ status }}</h2>
    </div>
    <div v-else-if="status === 'To Do'" class="bg-red-500 py-9 px-16 w-full">
      <h2 class="w-20 font-bold text-black">{{ status }}</h2>
    </div>
    <div v-else-if="status === 'In Progress'" class="bg-yellow-500 py-9 px-16 w-full">
      <h2 class="w-20 font-bold text-black">{{ status }}</h2>
    </div>
    <div v-else-if="status === 'Done'" class="bg-green-500 py-9 px-16 w-full">
      <h2 class="w-20 font-bold text-black">{{ status }}</h2>
    </div>

    <KanbanCard v-for="card in cards" :key="card.id" :card="card"></KanbanCard>
  </div>
</template>

<script setup>
import { defineProps, defineEmits } from 'vue'
import KanbanCard from './KanbanCard.vue'
const props = defineProps({
  status: String,
  cards: Array
})
const emit = defineEmits(['moveCard'])
const drop = (event) => {
  const cardId = event.dataTransfer.getData('text/plain')
  emit('moveCard', parseInt(cardId, 10), props.status)
}
</script>
