<script setup>
import { ref } from 'vue'

const props = defineProps({
  cards: Array
})
const open = ref(false)
const cards = props.cards
const newTodo = ref('')

const addTask = function() {
  cards.push({id: props.cards.slice(-1)[0].id++, title: newTodo.value, status: 'New' })
  newTodo.value = ''
  open.value = false
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
        <h2 class="text-center">Add a new task</h2>
        <div class="m-2">
          <input v-model='newTodo' placeholder="Task" >
        </div>
        <button class="text-black justify-start m-5 pointer-events-auto rounded-md bg-red-600 px-3 py-2 text-[0.8125rem] font-semibold leading-5 text-white hover:bg-indigo-500" @click="open = false">Close</button>
        <button class="text-black justify-end m-5 pointer-events-auto rounded-md bg-green-600 px-3 py-2 text-[0.8125rem] font-semibold leading-5 text-white hover:bg-indigo-500" @click="addTask">Add task</button>
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
