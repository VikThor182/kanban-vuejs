<template>
  <div
    class="ml-2 text-center my-2 kanban-card w-48 text-black bg-gray-100 rounded shadow p-5 mb-2 cursor-pointer"
    draggable="true"
    @dragstart="dragStart($event, card)"
  >
    {{ card.title }}
  </div>
</template>

<script setup>
import { defineProps } from 'vue'
const props = defineProps({
  card: Object
})
const dragStart = (event, card) => {
  event.dataTransfer.setData('text/plain', card.id)
}
</script>
