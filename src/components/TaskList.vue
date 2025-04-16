<script setup lang="ts">
import type { Task } from "../types";
const props = defineProps<{
  tasks: Task[];
}>();

const emits = defineEmits<{
  toggleDone: [id: string];
  removeTask: [id: string];
}>();
</script>
<template>
  <TransitionGroup name="list" tag="div" class="taks-list">
    <article v-for="task in props.tasks" class="task" :key="task.id">
      <input
        type="checkbox"
        :checked="task.done"
        @input="emits('toggleDone', task.id)"
      />
      <span :class="{ done: task.done }">{{ task.title }}</span>
      <button class="outline" @click="emits('removeTask', task.id)">
        Remove
      </button>
    </article>
  </TransitionGroup>
</template>
<style scoped>
.task-list {
  margin-top: 1rem;
}
.done {
  text-decoration: line-through;
}
.task {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.list-enter-active,
.list-leave-active {
  transition: all 0.5s ease;
}
.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateX(30px);
}
</style>
