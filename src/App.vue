<script setup lang="ts">
import { computed, ref } from "vue";
import TaskForm from "./components/TaskForm.vue";
import type { Task, TaskFilter } from "./types";
import TaskList from "./components/TaskList.vue";
import FilterButton from "./components/FilterButton.vue";
import EditTaskModal from "./components/EditTaskModal.vue";

const message = ref("To Vued");
const tasksList = ref<Task[]>([]);
const filter = ref<TaskFilter>("all");
const taskToEdit = ref<string | null>(null);

const tasksCompleted = computed(
  () => tasksList.value.filter((t) => t.done).length,
);

const filteredTasks = computed(() => {
  switch (filter.value) {
    case "all":
      return tasksList.value;
    case "todo":
      return tasksList.value.filter((task) => !task.done);
    case "done":
      return tasksList.value.filter((task) => task.done);
  }
});

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

function removeTask(id: string) {
  tasksList.value = tasksList.value.filter((t) => t.id !== id);
}

function setFilter(newFilter: TaskFilter) {
  filter.value = newFilter;
}

function openEditTask(id: string) {
  taskToEdit.value = id;
}
const closeEditTask = () => {
  taskToEdit.value = null;
};

const updateTask = (id: string, newTitle: string) => {
  const task = tasksList.value.find((t: Task) => t.id === id);
  if (task) task.title = newTitle;
  closeEditTask();
};
</script>

<template>
  <main>
    <h1>{{ message }}</h1>
    <TaskForm @add-task="addTask" />
    <h3 v-if="!tasksList.length">Add tasks to get started.</h3>
    <h3 v-else>
      {{ tasksCompleted }} / {{ tasksList.length }} Tasks completed.
    </h3>
    <div v-if="tasksList.length" class="button-container">
      <FilterButton
        :current-filter="filter"
        filter="all"
        @set-filter="setFilter"
      />
      <FilterButton
        :current-filter="filter"
        filter="todo"
        @set-filter="setFilter"
      />
      <FilterButton
        :current-filter="filter"
        filter="done"
        @set-filter="setFilter"
      />
    </div>
    <TaskList
      :tasks-list="filteredTasks"
      @toggle-done="toggleDone"
      @remove-task="removeTask"
      @edit-task="openEditTask"
    />
  </main>
  <EditTaskModal
    v-if="taskToEdit"
    :task="tasksList.find((t: Task) => t.id === taskToEdit)!"
    @close="closeEditTask"
    @save="updateTask"
  />
</template>

<style>
main {
  max-width: 800px;
  margin: 1rem auto;
}
.button-container {
  display: flex;
  justify-content: end;
  gap: 0.5rem;
}

.button-container button {
  height: 2rem;
  display: flex;
  align-items: center;
}
</style>
