<script setup>
import { ref } from 'vue';

const showModal = ref(false);
const newNote = ref("");
const error = ref("");
const notes = ref([{
  id: Math.floor(Math.random() * 100000),
  text: 'You can create a new note by clicking on the button add (+)',
  color: 'rgb(226, 128, 255)',
  date: new Date()
},])

function getRandomColor() {
  const color = "hsl(" + Math.random() * 360 + ", 100%, 75%)";
  return color;
}

const addNote = () => {
  if (newNote.value.length < 10) {
    return error.value = "Note must contain at least 10 letters or more"
  }
  notes.value.push({
    id: Math.floor(Math.random() * 100000),
    text: newNote.value,
    color: getRandomColor(),
    date: new Date()
  })
  showModal.value = false;
  newNote.value = ""
  error.value = ""
}
</script>

<template>
  <main class="h-screen w-screen">
    <div v-show="showModal" class="overlay absolute w-full h-full bg-black/80 z-10 flex items-center justify-center">
      <div class="w-[650px] mb-4 border p-5 border-gray-200 rounded-lg bg-gray-50 dark:bg-gray-700 dark:border-gray-700">
        <div class="px-4 py-2 bg-white rounded-t-lg dark:bg-gray-800">
          <label for="comment" class="sr-only">Your comment</label>
          <textarea id="comment" v-model="newNote" rows="4"
            class="w-[580px] px-0 text-sm text-gray-900 outline-none bg-white border-0 dark:bg-gray-800 focus:ring-0 dark:text-white dark:placeholder-gray-400"
            placeholder="Write a comment..." required></textarea>
          </div>
          <p v-show="error" class="text-red-400">{{ error }}</p>
        <div class="flex items-center justify-left pt-5 order-t dark:border-gray-600">
          <button @click="addNote" type="submit"
            class="inline-flex items-center mr-2 py-2.5 px-4 text-xs font-medium text-center text-white bg-blue-700 rounded-lg focus:ring-4 focus:ring-blue-200 dark:focus:ring-blue-900 hover:bg-blue-800">
            Post comment
          </button>
          <button @click="showModal = false"
            class="inline-flex items-center py-2.5 px-4 text-xs font-medium text-center text-white bg-red-500 rounded-lg focus:ring-4 focus:ring-red-200 dark:focus:ring-red-700 hover:bg-red-600">
            close
          </button>
        </div>
      </div>
    </div>
    <div class="container max-w-5xl p-3 my-0 mx-auto">
      <header class="flex justify-between items-center mb-5">
        <h1 class="font-bold mb-5 text-5xl text-gray-900">Notes</h1>
        <button @click="showModal = true"
          class="border-none p-2 w-11 h-11 cursor-pointer bg-gray-900 rounded-full text-white text-xl">+</button>
      </header>
      <div class="card-container flex flex-wrap">
        <div v-for="note in notes" key="note.id" :style="{ backgroundColor: note.color }"
          class="card w-56 h-56 bg-yellow-300 p-3 rounded-2xl flex flex-col justify-between mr-5 mb-5">
          <p class="main-text text-base font-semibold">{{ note.text }}</p>
          <p class="date text-xs mt-auto">{{ note.date.toLocaleDateString("en-US") }}</p>
        </div>
      </div>
    </div>
  </main>
</template>