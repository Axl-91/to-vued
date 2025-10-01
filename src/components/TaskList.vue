<script setup lang="ts">
import type { Task } from "../types";

const props = defineProps<{
  tasksList: Task[];
}>();

const emits = defineEmits<{
  toggleDone: [id: string];
}>();
</script>

<template>
  <div class="task-list">
    <article v-for="task in props.tasksList" :key="task.id">
      <label>
        <input
          @input="emits('toggleDone', task.id)"
          :checked="task.done"
          type="checkbox"
        />
        <span :class="{ done: task.done }">{{ task.title }}</span>
      </label>
    </article>
  </div>
</template>

<style scoped>
.task-list {
  margin-top: 1rem;
}

.done {
  text-decoration: line-through;
}
</style>
