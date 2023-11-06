<script setup>
import { Icon } from '@iconify/vue'

const prop = defineProps({
  todo: {
    type: Object,
    required: true
    // default: 1
    // default() {
    //     return {

    //     }
    // }
  },
  index: {
    type: Number,
    required: true
  }
})

defineEmits(['toggle-complete', 'edit-todo', 'update-todo', 'delete-todo'])
</script>

<template>
  <li class="group flex items-center gap-2 px-3 py-4 bg-[#f1f1f1] shadow-md">
    <input
      class="appearance-none checked:bg-[#41b080] w-5 h-5 bg-white rounded-full shadow-sm"
      type="checkbox"
      :checked="todo.isCompleted"
      @input="$emit('toggle-complete', index)"
    />
    <div class="flex">
      <input
        v-if="todo.isEditing"
        class="w-full px-2 py-1 border-2 border-[#41b080]"
        type="text"
        :value="todo.todo"
        @input="$emit('update-todo', $event.target.value, index)"
      />
      <span v-else :class="{ 'line-through': todo.isCompleted }">{{ todo.todo }}</span>
    </div>
    <div class="flex gap-1 opacity-0 group-hover:opacity-100 transition-opacity duration-150">
      <Icon
        v-if="todo.isEditing"
        icon="ph:check-circle"
        class="cursor-pointer check-icon"
        color="41b080"
        width="22"
        @click="$emit('edit-todo', index)"
      />
      <Icon
        v-else
        icon="ph:pencil-fill"
        class="cursor-pointer edit-icon"
        color="41b080"
        width="22"
        @click="$emit('edit-todo', index)"
      />
      <Icon
        icon="ph:trash"
        class="cursor-pointer trash-icon"
        color="f95e5e"
        width="22"
        @click="$emit('delete-todo', todo.id)"
      />
    </div>
  </li>
</template>
