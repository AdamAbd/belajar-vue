<script setup>
import { ref, watch, computed } from 'vue'
import { uid } from 'uid'
import { Icon } from '@iconify/vue'

import TodoCreator from '../components/TodoCreator.vue'
import TodoItem from '../components/TodoItem.vue'

const todoList = ref([])

watch(
  todoList,
  () => {
    setTodoListLocalStorage()
  },
  { deep: true }
)

const todosCompleted = computed(() => {
  return todoList.value.every((todo) => todo.isCompleted)
})

const fetchTodoList = () => {
  const saveTodoList = JSON.parse(localStorage.getItem('todoList'))

  if (saveTodoList) {
    todoList.value = saveTodoList
  }
}

fetchTodoList()

const setTodoListLocalStorage = () => {
  localStorage.setItem('todoList', JSON.stringify(todoList.value))
}

const createTodo = (todo) => {
  todoList.value.push({
    id: uid(),
    todo,
    isCompleted: null,
    isEditing: null
  })
}

const toggleTodoComplete = (todoPos) => {
  todoList.value[todoPos].isCompleted = !todoList.value[todoPos].isCompleted
}

const toggleEditTodo = (todoPos) => {
  todoList.value[todoPos].isEditing = !todoList.value[todoPos].isEditing
}

const updateTodo = (todoVal, todoPos) => {
  todoList.value[todoPos].todo = todoVal
}

const deleteTodo = (todoId) => {
  todoList.value = todoList.value.filter((todo) => todo.id !== todoId)
}
</script>

<template>
  <main class="flex flex-col max-w-lg mx-auto w-full px-10 py-4">
    <h1 class="mb-4 text-center text-xl font-medium">Create Todo</h1>
    <TodoCreator @create-todo="createTodo" />
    <ul v-if="todoList.length > 0" class="flex flex-col mt-6 gap-5">
      <TodoItem
        v-for="(todo, index) in todoList"
        :key="todo.id"
        :todo="todo"
        :index="index"
        @toggle-complete="toggleTodoComplete"
        @edit-todo="toggleEditTodo"
        @update-todo="updateTodo"
        @delete-todo="deleteTodo"
      />
    </ul>
    <p v-else class="flex items-center justify-center gap-2 mt-6">
      <Icon icon="noto-v1:sad-but-relieved-face" />
      <span>You have no todo's to complete! Add one!</span>
    </p>
    <p
      v-if="todosCompleted && todoList.length > 0"
      class="flex items-center justify-center gap-2 mt-6"
    >
      <Icon icon="noto-v1:party-popper" />
      <span>You have completed all your todos!</span>
    </p>
  </main>
</template>
