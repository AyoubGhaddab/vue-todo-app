<script lang="ts" setup>
import { ref } from "vue";
const message = ref("hello world");
const newTask = ref("");
const error = ref("");
const emit = defineEmits<{
  addTask: [newTask: string];
}>();
function formSubmit(event: Event) {
  if (!newTask.value.trim()) {
    error.value = "Task cannot be empty";
    return;
  }
  emit("addTask", newTask.value.trim());
  newTask.value = "";
  error.value = "";
}
</script>
<template>
  <form @submit.prevent="formSubmit">
    <label>
      New Task
      <input
        v-model="newTask"
        type="text"
        name="newTask"
        :aria-invalid="!!error || undefined"
        @input="error = ''"
      />
      <small v-if="error" id="invalid-helper">
        {{ error }}
      </small>
    </label>
    <div class="button-container">
      <button type="submit">Add Task</button>
    </div>
  </form>
</template>
<style lang="css" scoped>
.button-container {
  display: flex;
  justify-content: end;
}
</style>
