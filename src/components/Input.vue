<script setup lang="ts">
defineProps<{ label: string }>();
</script>

<template>
  <label class="flex flex-col gap-1">
    <span>{{ label }}</span>
    <input v-bind="$attrs" required class="p-2 rounded text-slate-800" />
  </label>
</template>
