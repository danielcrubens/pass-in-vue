<template>
  <button :class="buttonClass" v-bind="$attrs">
    <slot></slot>
  </button>
</template>

<script setup>
import { defineProps } from 'vue';

const props = defineProps({
  transparent: Boolean
});

const buttonClass = {
  'bg-black/20 border border-white/10 rounded-md p-1.5': props.transparent,
  'bg-white/10 border border-white/10 rounded-md p-1.5': !props.transparent
};
</script>
