<script lang="ts" setup>
import { computed, ref } from 'vue';
import TodoForm from './components/TodoForm.vue';
import TodoList from './components/TodoList.vue';
import FilterButton from './components/FilterButton.vue';
import type { Todo, TodoFilterType } from './types';

const todos = ref<Todo[]>([]);
const filter = ref<TodoFilterType>('all');

const completedTodosCount = computed(() => {
  return todos.value.reduce(
    (count, todo) => count + (todo.completed ? 1 : 0),
    0
  );
});

const filteredTodos = computed(() => {
  if (filter.value === 'all') {
    return todos.value;
  }
  if (filter.value === 'completed') {
    return todos.value.filter((todo) => todo.completed);
  }
  return todos.value.filter((todo) => !todo.completed);
});

function addTodo(todo: string) {
  todos.value.push({
    id: crypto.randomUUID(),
    title: todo,
    completed: false,
  });
}

function removeTodo(id: string) {
  const index = todos.value.findIndex((todo) => todo.id === id);
  if (index !== -1) {
    todos.value.splice(index, 1);
  }
}

function markAsCompleted(id: string) {
  const todo = todos.value.find((todo) => todo.id === id);
  if (todo) {
    todo.completed = !todo.completed;
  }
}

function setFilter(type: TodoFilterType) {
  filter.value = type;
}
</script>

<template>
  <main>
    <h1>Todo</h1>

    <TodoForm @add-todo="addTodo" />

    <h3 v-if="!todos.length">Add a todo to get started!</h3>
    <h3 v-else>
      {{ completedTodosCount }} / {{ todos.length }} todos completed.
    </h3>

    <div role="group">
      <FilterButton
        filter="all"
        :active="filter === 'all' ? true : false"
        @set-filter="setFilter"
      />
      <FilterButton
        filter="todo"
        :active="filter === 'todo' ? true : false"
        @set-filter="setFilter"
      />
      <FilterButton
        filter="completed"
        :active="filter === 'completed' ? true : false"
        @set-filter="setFilter"
      />
    </div>

    <TodoList
      :todos="filteredTodos"
      @mark-as-completed="markAsCompleted"
      @remove-todo="removeTodo"
    />
  </main>
</template>

<style>
main {
  max-width: 800px;
  margin: 1rem auto;
}

.button-wrapper {
  display: flex;
  justify-content: end;
}

.done {
  text-decoration: line-through;
}
</style>
