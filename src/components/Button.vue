<script setup lang="ts">
defineProps<{ variant?: "delete" | "done" }>();
</script>

<template>
  <button :class="variant && $style[variant]" v-bind="$attrs">
    <slot></slot>
  </button>
</template>

<style module>
button {
  @apply text-black p-2 rounded uppercase bg-amber-300 hover:scale-105 transition-transform duration-100 active:scale-95;
}
.delete {
  @apply bg-rose-700 text-white;
}
.done {
  @apply bg-emerald-700 text-white;
}
</style>
