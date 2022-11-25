<script setup>

import { ref, onMounted, computed, watch } from 'vue'


const todos = ref([])
const name = ref('')
const input_content = ref('')
const input_category = ref(null)

const todos_asc = computed(() => todos.value.sort((a, b) => {
  return a.createdAt - b.createdAt;
}))

//addTodo

const addTodo = ()=>{
  if(input_content.value.trim() ==="" || input_category ===null){
    return 
  }  

  todos.value.push({
    content :input_content.value,
    category:input_category.value,
    done:false,
    createdAt: new Date().getTime(),
  })



}

// save todo

watch(todos , (newVal)=>{
  localStorage.setItem('todos', JSON.stringify(newVal));
},{ deep : true })




// watch

watch(name, (newValue) => {
  localStorage.setItem('name', newValue)
})

// get

onMounted(() => {
  name.value = localStorage.getItem('name') || '';
  todos.value = JSON.parse(localStorage.getItem('todos')) || [];
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

        <!-- {{ input_content }} -->

        <h4>Pick up your category</h4>

        <div class="options">
          <label>
            <input type="radio" name="category" value="business" v-model="input_category" />
            <span class="bubble business"></span>
            <div>Business</div>
          </label>
          <label>
            <input type="radio" name="category" value="personal" v-model="input_category" />
            <span class="bubble personal"></span>
            <div>personal</div>
          </label>

          <!-- {{input_category}} -->

        </div>

        <input type="submit" value="add todo">

      </form>
    </section>

   <!-- todo list -->

   <section class="todo-list">

    <h3>Todo list </h3>

    <div class="list">
      <div v-for="todo in todos_asc" :class="`todo-item ${todo.done && 'done'}`" >
          <label>
            <input type="checkbox"  v-model="todo.done"/>
            <span :class="`bubble ${todo.category}`"> </span>
          </label>

          <div class="todo_content">
            <input type="text" v-model="todo.content">
          </div>


      </div>
    </div>

   </section>

  </main>


</template>

