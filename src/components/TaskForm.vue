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
      <div class="input-container">
        <input
          v-model="newTask"
          name="newTask"
          :aria-invalid="!!taskError || undefined"
          @input="taskError = ''"
          placeholder="Add some task..."
        />
        <button>Add</button>
      </div>
      <small v-if="taskError" id="invalid-helper"> {{ taskError }} </small>
    </label>
  </form>
</template>

<style scoped>
small {
  padding-left: 0.5rem;
  color: rgb(192, 1, 1);
}
.input-container {
  display: flex;
  align-items: center;
  padding: 5px;
  gap: 5px;
  margin-bottom: 0;
}
.input-container input {
  height: 2rem;
  margin: 0;
}
.input-container button {
  height: 2rem;
  display: flex;
  align-items: center;
}
</style>
