<script setup>
import { ref } from 'vue'
import axios from 'axios'

const props = defineProps({
  cards: Array,
  getDataTasks: Function
})

const emit = defineEmits(['taskAdded'])
const open = ref(false)
const cards = ref([props.cards])
const newTodoTitle = ref('')
const newTodoDescription = ref('')
const url = '"'+import.meta.env.VITE_API_URI+'"'
const idTask = ref(0);
console.log('idTask = '+idTask.value)
console.log(cards.value[0])

if (cards.value[0].length > 0) {
  idTask.value = cards.value.slice(-1)[0].id++
}

async function addTask() {
  axios.post(url, {
    id: idTask,
    title: newTodoTitle.value,
    status: 'New',
    description: newTodoDescription.value
  })
  .then(function (response) {
    newTodoTitle.value = ''
    newTodoDescription.value = ''
    open.value = false
    emit('taskAdded', response);
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
    newTodoTitle.value = ''
    newTodoDescription.value = ''
    open.value = false
  });
}

</script>
<template>
  <div class="w-auto bg-gray-100 flex justify-end">
    <button @click="open = true"
      class="text-black justify-end m-5 pointer-events-auto rounded-md bg-indigo-600 px-3 py-2 text-[0.8125rem] font-semibold leading-5 text-white hover:bg-indigo-500"
    >
      Add Task
    </button>
    <Teleport to="body">
      <div v-if="open" class="modal">
        <form @submit.prevent="addTask">
          <h2 class="text-center">Add a new task</h2>
          <div class="m-2">
            <input v-model='newTodoTitle' placeholder="Task title" >
            <input v-model='newTodoDescription' placeholder="Task description (if needed)" >
          </div>
          <button class="text-black justify-start m-5 pointer-events-auto rounded-md bg-red-600 px-3 py-2 text-[0.8125rem] font-semibold leading-5 text-white hover:bg-indigo-500" @click="open = false">Close</button>
          <button class="text-black justify-end m-5 pointer-events-auto rounded-md bg-green-600 px-3 py-2 text-[0.8125rem] font-semibold leading-5 text-white hover:bg-indigo-500" type="submit">Add task</button>
        </form>      
      </div>
    </Teleport>
  </div>
</template>
<style scoped>
.modal {
  position: fixed;
  z-index: 999;
  top: 20%;
  left: 35%;
  width: 500px;
  margin-left: -150px;
  border: 1px, black, solid;
  border-radius: 5px;
  background-color: white;
  color:black
}
</style>
