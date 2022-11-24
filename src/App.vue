<script setup>
  import { ref, onMounted, computed, watch } from 'vue'
  const todos = ref([])
  const name = ref('')
  const input_content = ref('')

  const todos_asc = computed(() => todos.value.sort((a,b) =>{
    return a.createdAt - b.createdAt
  }))
  
  watch(todos, (newVal) => {
    localStorage.setItem('todos', JSON.stringify(newVal))
  }, {
    deep: true
  })
  const addTodo = () => {
    if (input_content.value.trim() === '') {
      return
    }
    todos.value.push({
      content: input_content.value,
      done: false,
      editable: false,
      createdAt: new Date().getTime()
    })
  }
  const removeTodo = (todo) => {
    todos.value = todos.value.filter((t) => t !== todo)
  }
  onMounted(() => {
    todos.value = JSON.parse(localStorage.getItem('todos')) || []
  })
</script>

<template>
  <main class="app">
    <section class="create-todo">
      <h1>Create To-do</h1>
      <form id="new-todo-form" @submit.prevent="addTodo">
        <input type="text" name="content" id="content" placeholder="Put me in" v-model="input_content"/>
        <input type="submit" value="Add todo" />
      </form>
    </section>
    <section class="todo">
      <h1>My List</h1>
      <div class="list" id="todo-list">
        <div v-for="todo in todos_asc" :class="`todo-item ${todo.done && 'done'}`">
          <input type="checkbox" v-model="todo.done" />
          <div class="todo-content">
						<input type="text" v-model="todo.content" />
					</div>

					<div class="actions">
						<button class="delete" @click="removeTodo(todo)">Delete</button>
					</div>
        </div>

      </div>
    </section>
  </main>
</template>

