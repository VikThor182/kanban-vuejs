<template>
  <div class="flex justify-around w-full p-5 bg-gray-100 min-h-screen">
    <KanbanColumn
      v-for="status in columnStatuses"
      :key="status"
      :status="status"
      :cards="cards.filter((card) => card.status === status)"
      @moveCard="moveCard"
    ></KanbanColumn>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import KanbanColumn from './KanbanColumn.vue'
const cards = ref([
  { id: 1, title: 'Task 1', status: 'New' },
  { id: 2, title: 'Task 2', status: 'To Do' }
])
const columnStatuses = ['New', 'To Do', 'In Progress', 'Done']
const moveCard = (cardId, newStatus) => {
  const card = cards.value.find((card) => card.id === cardId)
  if (card) {
    card.status = newStatus
  }
}
</script>
