<script setup lang="ts">
import { TransitionGroup } from "vue";
import type { Task } from "../types";

const props = defineProps<{
  tasksList: Task[];
}>();

const emits = defineEmits<{
  toggleDone: [id: string];
  removeTask: [id: string];
  editTask: [id: string];
}>();
</script>

<template>
  <TransitionGroup name="task-article" tag="div" class="task-list">
    <article v-for="task in props.tasksList" :key="task.id" class="task">
      <label>
        <input
          @input="emits('toggleDone', task.id)"
          :checked="task.done"
          type="checkbox"
        />
        <span :class="{ done: task.done }">{{ task.title }}</span>
      </label>
      <div class="button-container">
        <button class="outline e-btn" v-on:click="emits('editTask', task.id)">
          Edit
        </button>
        <button class="outline d-btn" v-on:click="emits('removeTask', task.id)">
          Delete
        </button>
      </div>
    </article>
  </TransitionGroup>
</template>

<style scoped>
.e-btn {
  color: #12cf0f;
  border-color: #12cf0f;
}

.e-btn:hover {
  color: #71e26f;
  border-color: #71e26f;
}

.d-btn {
  color: #c90000;
  border-color: #c90000;
}

.d-btn:hover {
  color: #e66666;
  border-color: #e66666;
}

.task-list {
  margin-top: 1rem;
}

.task {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.done {
  text-decoration: line-through;
}

.task-article-enter-active,
.task-article-leave-active {
  transition: all 0.5s ease;
}
.task-article-enter-from,
.task-article-leave-to {
  opacity: 0;
  transform: translateX(300px);
}
</style>
