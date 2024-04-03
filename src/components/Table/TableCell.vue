<template>
  <td v-bind="attributes" :class="mergedClasses">
    <slot></slot>
  </td>
</template>

<script setup>
import { defineProps } from 'vue';
import { twMerge } from 'tailwind-merge';

const props = defineProps();

// Extrai as propriedades de `props` e remove `className` para evitar conflitos
const { className, ...attributes } = props;

const mergedClasses = twMerge(
  'py-3 px-4 text-sm text-zinc-300', // Adiciona classes padrão do Tailwind CSS
  className // Adiciona classes passadas diretamente para o componente
);
</script>
