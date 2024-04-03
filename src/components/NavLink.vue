<template>
  <a :href="to" class="font-medium text-sm text-zinc-300">
    <slot></slot>
  </a>
</template>

<script setup>
import { defineProps } from 'vue';

const props = defineProps({
  to: {
    type: String,
    required: true
  }
});
</script>
