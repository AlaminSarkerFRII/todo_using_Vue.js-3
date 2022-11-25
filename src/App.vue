<script setup>

import { ref, onMounted, computed, watch } from 'vue'


const todos = ref([])
const name = ref('')
const input_content = ref('')
const input_category = ref(null)

const todos_asc = computed(() => todos.value.sort((a, b) => {
  return b.createdAt - a.createdAt;
}))


// watch

watch(name, (newValue) => {
  localStorage.setItem('name', newValue)
})

// get

onMounted(() => {
  name.value = localStorage.getItem('name') || '';
})


</script>

<template>

  <main class="app">
    <section class="greeting">
      <h2 class="title">
        whats up <input type="text" placeholder="type name" />
      </h2>
    </section>

    <section class="create-todo">
      <h3>CREATE A TODO</h3>
      <form @submit.prevent="addTodo">
        <h4>Whats on your todo list ?</h4>

        <input type="text" placeholder="add your video" v-model="input_content">
        
        {{ input_content }}

      </form>
    </section>

  </main>


</template>

