<script setup lang="ts">
import { TransitionGroup } from "vue";
import type { Task } from "../types";

const props = defineProps<{
  tasksList: Task[];
}>();

const emits = defineEmits<{
  toggleDone: [id: string];
  removeTask: [id: string];
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
      <button class="outline" v-on:click="emits('removeTask', task.id)">
        Delete
      </button>
    </article>
  </TransitionGroup>
</template>

<style scoped>
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
