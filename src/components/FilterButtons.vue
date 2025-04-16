<script setup lang="ts">
import type { TaskFilter } from "../types";
const props = defineProps<{
  filter: TaskFilter;
  currentFilter: TaskFilter;
}>();
const emits = defineEmits<{
  filter: [filter: TaskFilter];
}>();
</script>

<template>
  <button
    class="secondary"
    @click="emits('filter', filter)"
    :class="{ contrast: props.currentFilter === props.filter }"
  >
    {{ props.filter }}
  </button>
</template>

<style lang="css" scoped></style>
