<script setup lang="ts">
import { computed, ref } from "vue";
import TaskForm from "./components/TaskForm.vue";
import type { Task } from "./types";
import TaskList from "./components/TaskList.vue";

const message = ref("To Vued");
const tasksList = ref<Task[]>([]);

const tasksCompleted = computed(
  () => tasksList.value.filter((t) => t.done).length
);

function addTask(newTask: string) {
  tasksList.value.push({
    id: crypto.randomUUID(),
    title: newTask,
    done: false,
  });
}

function toggleDone(id: string) {
  const task = tasksList.value.find((t) => t.id === id);
  if (task) task.done = !task.done;
}
</script>

<template>
  <main>
    <h1>{{ message }}</h1>
    <TaskForm @add-task="addTask" />
    <h3 v-if="!tasksList.length">Add tasks to get started.</h3>
    <h3 v-else>
      {{ tasksCompleted }} / {{ tasksList.length }} Tasks completed.
    </h3>
    <TaskList :tasksList @toggle-done="toggleDone" />
  </main>
</template>

<style scoped>
main {
  max-width: 800px;
  margin: 1rem auto;
}
</style>
