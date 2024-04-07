<template>
  <tr class="border-b border-white/10 hover:bg-white/15">
    <slot></slot>
  </tr>
</template>

<script setup>
import { defineProps } from 'vue';

const props = defineProps();
</script>
