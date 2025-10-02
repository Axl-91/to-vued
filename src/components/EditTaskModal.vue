<script setup lang="ts">
import { onMounted, ref } from "vue";
import type { Task } from "../types";
import { motion } from "motion-v";

const props = defineProps<{
  task: Task;
}>();

const emits = defineEmits<{
  close: [void];
  save: [id: string, newTitle: string];
}>();

const newTitle = ref(props.task.title);
const inputRef = ref<HTMLInputElement | null>(null);

const save = () => {
  emits("save", props.task.id, newTitle.value);
};

onMounted(() => {
  inputRef.value?.focus();
});
</script>

<template>
  <div class="modal-overlay">
    <div class="modal-content">
      <h2>Edit Task</h2>
      <form @submit.prevent="save">
        <input ref="inputRef" v-model="newTitle" type="text" autofocus />

        <div class="buttons">
          <motion.button
            :whilePress="{ scale: 0.95 }"
            type="button"
            @click="$emit('close')"
            >Cancel</motion.button
          >
          <motion.button :whilePress="{ scale: 0.95 }" type="submit"
            >Save</motion.button
          >
        </div>
      </form>
    </div>
  </div>
</template>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.5); /* semi-transparent */
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 9999;
}

input {
  height: 2rem;
}

.modal-content {
  background: rgb(19, 22.5, 30.5);
  padding: 20px;
  border-radius: 8px;
  width: 400px;
  max-width: 90%;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
}

.buttons {
  display: flex;
  justify-content: flex-end;
  gap: 10px;
}
.buttons button {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 2rem;
  width: 4rem;
}
</style>
