<script lang="ts" setup>
import { ref } from 'vue';

const emits = defineEmits<{
  addTodo: [newTodo: string];
}>();

const newTodo = ref('');
const error = ref('');

function submit() {
  if (!newTodo.value.trim()) {
    error.value = 'Please enter a todo.';
    return;
  }

  emits('addTodo', newTodo.value.trim());
  newTodo.value = '';
}
</script>

<template>
  <form @submit.prevent="submit">
    <label>New Todo</label>
    <input
      v-model="newTodo"
      name="newTodo"
      :aria-invalid="!!error || undefined"
      @input="error = ''"
    />
    <small v-if="error" id="invalid-helper">{{ error }}</small>
    <div class="button-wrapper">
      <button>Add</button>
    </div>
  </form>
</template>
