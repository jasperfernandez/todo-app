<script setup lang="ts">
import type { TodoFilterType } from '../types';

const props = defineProps<{
  filter: TodoFilterType;
  active?: boolean;
}>();

const emits = defineEmits<{
  setFilter: [type: TodoFilterType];
}>();
</script>

<template>
  <button
    :class="{ primary: props.active, secondary: !props.active }"
    @click="emits('setFilter', props.filter)"
  >
    {{ props.filter }}
  </button>
</template>

<style scoped>
button {
  text-transform: capitalize;
}
</style>
