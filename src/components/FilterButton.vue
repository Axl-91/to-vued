<script lang="ts" setup>
import type { TaskFilter } from "../types";
import {motion} from 'motion-v'

const props = defineProps<{
  filter: TaskFilter;
  currentFilter: TaskFilter;
}>();

const emit = defineEmits<{
  setFilter: [filter: TaskFilter];
}>();
</script>

<template>
  <motion.button
    :whileHover="{scale: 1.10}"
    :whilePress="{scale: 0.95}"
    :class="{ contrast: props.currentFilter === props.filter }"
    @click="emit('setFilter', props.filter)"
  >
    {{ props.filter }}
  </motion.button>
</template>

<style scoped>
button {
  text-transform: capitalize;
}
</style>
