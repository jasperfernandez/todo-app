<script lang="ts" setup>
import type { Todo } from '../types';

defineProps<{
  todos: Todo[];
}>();

const emits = defineEmits<{
  markAsCompleted: [id: string];
  removeTodo: [id: string];
}>();
</script>

<template>
  <TransitionGroup name="todo-list" tag="div" class="todo-list">
    <article v-for="todo in todos" :key="todo.id" class="todo">
      <label>
        <input
          @input="emits('markAsCompleted', todo.id)"
          :checked="todo.completed"
          type="checkbox"
        />
        <span :class="{ done: todo.completed }">{{ todo.title }}</span>
      </label>

      <button class="outline" @click="emits('removeTodo', todo.id)">
        Remove
      </button>
    </article>
  </TransitionGroup>
</template>

<style>
.todo {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.todo-list-enter-active,
.todo-list-leave-active {
  transition: all 0.5s ease;
}

.todo-list-enter-from,
.todo-list-leave-to {
  opacity: 0;
  transform: translateX(300px);
}
</style>
