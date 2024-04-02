<template>
  <div class="flex items-center gap-5 py-2">
    <img :src="logo" :alt="alt" />
    <nav class="flex items-center gap-5">
      <a class="font-medium text-sm text-zinc-300" href="">Eventos</a>
      <a class="font-medium text-sm" href="">Participantes</a>
    </nav>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import logoImage from '../assets/logo.svg'

const logo = ref(logoImage)
const alt = ref('Logo pass-in')
</script>
