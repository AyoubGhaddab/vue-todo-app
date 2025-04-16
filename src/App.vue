<script lang="ts" setup>
import { computed, ref } from "vue";
import TaskForm from "./components/TaskForm.vue";
import TaskList from "./components/TaskList.vue";
import FilterButtons from "./components/FilterButtons.vue";
import type { Task, TaskFilter } from "./types";

const tasks = ref<Task[]>([]);
const totalDone = computed(() => {
  return tasks.value.reduce(
    (total, task) => (task.done ? total + 1 : total),
    0
  );
});
const filter = ref<TaskFilter>("all");
const filteredTasks = computed(() => {
  switch (filter.value) {
    case "all":
      return tasks.value;
    case "todo":
      return tasks.value.filter((t) => !t.done);
    case "done":
      return tasks.value.filter((t) => t.done);
    default:
      return tasks.value;
  }
});
function addTask(newTask: string) {
  tasks.value.push({
    id: crypto.randomUUID(),
    title: newTask,
    done: false,
  });
}
function removeTask(id: string) {
  const index = tasks.value.findIndex((t) => t.id === id);
  if (index !== -1) {
    tasks.value.splice(index, 1);
  }
}
function toggleDone(id: string) {
  const task = tasks.value.find((t) => t.id === id);
  if (task) {
    task.done = !task.done;
  }
}
</script>

<template>
  <main>
    <TaskForm @add-task="addTask" />
    <h3 v-if="tasks.length === 0">Add a Task to get started</h3>
    <h3 v-else>{{ totalDone }} / {{ tasks.length }} tasks completed</h3>
    <div class="button-container">
      <FilterButtons
        :currentFilter="filter"
        filter="all"
        @filter="(f) => (filter = f)"
      />
      <FilterButtons
        :currentFilter="filter"
        filter="todo"
        @filter="(f) => (filter = f)"
      />
      <FilterButtons
        :currentFilter="filter"
        filter="done"
        @filter="(f) => (filter = f)"
      />
    </div>
    <TaskList
      @remove-task="removeTask"
      @toggle-done="toggleDone"
      :tasks="filteredTasks"
    />
  </main>
</template>
<style lang="css" scoped>
main {
  max-width: 800px;
  margin: 1rem auto;
}
.button-container {
  display: flex;
  justify-content: end;
  gap: 0.5rem;
  margin-bottom: 1rem;
}
</style>
