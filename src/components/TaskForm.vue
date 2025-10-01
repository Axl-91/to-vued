<script setup lang="ts">
import { ref } from "vue";

const emit = defineEmits<{
  addTask: [newTask: string];
}>();

const newTask = ref("");
const taskError = ref("");

function handleSubmit() {
  if (newTask.value.trim()) {
    emit("addTask", newTask.value);
  } else {
    taskError.value = "Task can't be empty";
  }
  newTask.value = "";
}
</script>

<template>
  <form @submit.prevent="handleSubmit" class="task-form">
    <label>
      New Task
      <input
        v-model="newTask"
        name="newTask"
        :aria-invalid="!!taskError || undefined"
        @input="taskError = ''"
      />
      <small v-if="taskError" id="invalid-helper"> {{ taskError }} </small>
    </label>
    <div class="button-container">
      <button>Add</button>
    </div>
  </form>
</template>

<style scoped>
input {
  height: 2rem;
}
</style>
