<template>
  <div class="flex items-center gap-5 py-2">
    <img :src="logo" :alt="alt" />
    <nav class="flex items-center gap-5">
      <NavLink to="eventos">Eventos</NavLink>
      <NavLink class="font-medium text-sm" to="participantes">Participantes</NavLink>
    </nav>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import logoImage from '../assets/logo.svg'
import NavLink from './NavLink.vue'

const logo = ref(logoImage)
const alt = ref('Logo pass-in')
</script>
