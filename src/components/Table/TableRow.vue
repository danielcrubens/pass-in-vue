<template>
  <tr class="border-b border-white/70 hover:bg-white/75">
    <slot></slot>
  </tr>
</template>

<script setup>
import { defineProps } from 'vue';

const props = defineProps();
</script>
