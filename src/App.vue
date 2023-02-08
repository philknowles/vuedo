<script setup>
  import { ref, onMounted, computed, watch } from 'vue'
  const todos = ref([])
  const name = ref('')

  const input_content = ref('')
  const input_category = ref(null)

  const todos_asc = computed(() => todos.value.sort((a,b) => {
    return a.createdAt - b.createdAt
  }))
  watch(name, (newVal) => {
    localStorage.setItem('name', newVal)
  })
  watch(todos, (newVal) => {
    localStorage.setItem('todos', JSON.stringify(newVal))
  }, {
    deep: true
  })
  const addTodo = () => {
    if (input_content.value.trim() === '' || input_category.value === null) {
	  return
	}
	todos.value.push({
		content: input_content.value,
		category: input_category.value,
		done: false,
		editable: false,
		createdAt: new Date().getTime()
	})
}
const removeTodo = (todo) => {
	todos.value = todos.value.filter((t) => t !== todo)
}
onMounted(() => {
	name.value = localStorage.getItem('name') || ''
	todos.value = JSON.parse(localStorage.getItem('todos')) || []
})
</script>

<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        What's Up, <input type="text" placeholder="Name here" v-model="name" />
      </h2>
    </section>

    <section class="create-todo">
      <h3>
        Create a Todo
      </h3>

      <form @submit.prevent="addTodo">
        <h4>
          What's on your todo list?
        </h4>

        <input type="text" placeholder="e.g. make a video" v-model="input_content" />

        <h4>Pick a category</h4>

        <div class="options">
          <label>
            <input type="radio" name="category" id="category1" value="Business" v-model="input_category" />
            <span class="bubble business"></span>
            <div>
              Business
            </div>
          </label>
          <label>
            <input type="radio" name="category" id="category1" value="Personal" v-model="input_category" />
            <span class="bubble personal"></span>
            <div>
              Personal
            </div>
          </label>

          <!-- {{ input_category }} -->
        </div>

        <input type="submit" value="Add Todo" />
      </form>
    </section>

    <section class="todo-list">
      <h3>Todo List</h3>

      <div class="list">
        <!-- <div v-for="todo in todo-asc" :class="`todo-item ${todo.done && 'done'}`">
          <label>
            <input type="checkbox" v-model="todo.done"/>
            <span class="`bubble ${todo.category`"></span>
          </label>
        </div> -->
      </div>
    </section>
  </main>
</template>
