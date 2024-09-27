<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'
import KanbanColumn from './KanbanColumn.vue'
import AddTaskButton from '../components/AddTaskButton.vue'

const cards = ref([])
const card = ref('')
const url = '"'+import.meta.env.VITE_API_URI+'"'

async function getDataTasks() {
  try {
    await axios.get(url).then(response => {  
      for (var i = 0; i < response.data.length; i++) {
        cards.value.push({id: response.data[i].id, title: response.data[i].fields.title, status: response.data[i].fields.status, description: response.data[i].fields.description})
      }
    })
  } catch(err) {
    console.log(err)
  }
}
async function addTaskToBoard(newTaskPromise) {
  try {
    const newTask = await newTaskPromise;
    console.log('before ' + cards.value)
    cards.value.push(JSON.stringify(newTask.config.data));
    console.log('after ' + JSON.stringify(cards.value))
  } catch (error) {
    console.error("Erreur lors de l'ajout de la tâche :", error);
  }
}
onMounted(() => {
  getDataTasks();
});
const columnStatus = ref([
  {id: 0, status:'New', color: 'bg-purple-500 py-9 px-16 w-full'},
  {id: 1, status:'Todo', color: 'bg-red-500 py-9 px-16 w-full'},
  {id: 2, status:'In progress', color: 'bg-yellow-500 py-9 px-16 w-full'},
  {id: 3, status:'Done', color: 'bg-green-500 py-9 px-16 w-full'}
])
const moveCard = (cardId, newStatus) => {
  card.value = cards.value.find((card) => card.id === cardId)
  if (card.value) {
    card.value.status = newStatus
  }
}

</script>
<template>
  <AddTaskButton :cards="cards" @taskAdded="addTaskToBoard"/>
  <div class="flex justify-around w-full p-5 bg-gray-100 min-h-screen">
    
    <KanbanColumn
      v-for="status in columnStatus"
      :key="status.id"
      :status="status.status"
      :color="status.color"
      :cards="cards.filter((card) => card.status === status.status)"
      :getDataTasks="getDataTasks"
      @moveCard="moveCard"
    ></KanbanColumn>
  </div>
</template>