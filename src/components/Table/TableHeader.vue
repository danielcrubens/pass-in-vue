<template>
    <th class="py-3 px-4 text-sm font-semibold text-left">
      <slot></slot>
    </th>
</template>

<script setup>
import { defineProps } from 'vue';

const props = defineProps();
</script>
