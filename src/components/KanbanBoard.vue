<script setup>
import { ref } from 'vue'
import KanbanColumn from './KanbanColumn.vue'
import AddTaskButton from '../components/AddTaskButton.vue'
const cards = ref([
  { id: 1, title: 'Task 1', status: 'New' },
  { id: 2, title: 'Task 2', status: 'Todo' }
])
const columnStatuses = ref([
  {id: 0, status:'New', color: 'bg-purple-500 py-9 px-16 w-full'},
  {id: 1, status:'Todo', color: 'bg-red-500 py-9 px-16 w-full'},
  {id: 2, status:'In progress', color: 'bg-yellow-500 py-9 px-16 w-full'},
  {id: 3, status:'Done', color: 'bg-green-500 py-9 px-16 w-full'}
])
const moveCard = (cardId, newStatus) => {
  const card = cards.value.find((card) => card.id === cardId)
  if (card) {
    card.status = newStatus
  }
}
</script>
<template>
  <AddTaskButton :cards="cards"/>
  <div class="flex justify-around w-full p-5 bg-gray-100 min-h-screen">
    
    <KanbanColumn
      v-for="status in columnStatuses"
      :key="status.id"
      :status="status.status"
      :color="status.color"
      :cards="cards.filter((card) => card.status === status.status)"
      @moveCard="moveCard"
    ></KanbanColumn>
  </div>
</template>
