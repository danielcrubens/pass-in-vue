<template>
  <div class="border border-white/10 rounded-lg">
    <table class="w-full" v-bind="$attrs">
      <slot></slot>
    </table>
  </div>
</template>

<script setup>
import { defineProps } from 'vue';

const props = defineProps();
</script>
