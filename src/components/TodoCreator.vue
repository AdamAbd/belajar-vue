<script setup>
import { reactive } from 'vue'
import TodoButton from './TodoButton.vue'

const emit = defineEmits(['create-todo'])

const todoState = reactive({
  todo: '',
  invalid: null,
  errMsg: ''
})

const createTodo = () => {
  todoState.invalid = false
  if (todoState.todo !== '') {
    emit('create-todo', todoState.todo)
    todoState.todo = ''
    return
  }
  todoState.invalid = true
  todoState.errMsg = 'Todo value cannot be empty!'
}
</script>

<template>
  <div
    class="flex border-2 border-[#41b080] transition-all duration-200 focus-within:shadow-xl"
    :class="{ 'border-red-500': todoState.invalid }"
  >
    <input class="w-full px-3 py-1 focus:outline-none" type="text" v-model="todoState.todo" />
    <TodoButton @click="createTodo()" />
  </div>
  <!-- <p v-if="todoState.invalid">{{ todoState.errMsg }}</p> -->
  <p class="mt-2 text-sm text-center text-red-500" v-show="todoState.invalid">
    {{ todoState.errMsg }}
  </p>
</template>
