<template>
  <div class="flex flex-col gap-10">
    <div class="flex gap-3 items-center">
      <h1 class="text-2xl font-bold">Participantes</h1>
      <div class="px-3 w-72 py-1.5 border border-white/10 b rounded-lg text-sm flex items-center gap-3">
        <Search class="text-emerald-300" :size="16" />
        <input class="bg-transparent flex-1 outline-none border-0 p-0 text-sm ring-0 accent-orange-400" placeholder="Buscar participante..." />
      </div>
    </div>
    <div class="border border-white/10 rounded-lg">
      <table class="w-full">
        <thead>
          <tr class="border-b border-white/10">
            <th class="py-3 px-4 text-sm font-semibold text-left w-11">
              <input class="size-4 bg-black/20 rounded border border-white/10 accent-orange-400"
                type="checkbox" />
            </th>
            <th class="py-3 px-4 text-sm font-semibold text-left">Código</th>
            <th class="py-3 px-4 text-sm font-semibold text-left">Participante</th>
            <th class="py-3 px-4 text-sm font-semibold text-left">Data de inscrição</th>
            <th class="py-3 px-4 text-sm font-semibold text-left">Data do check-in</th>
            <th :style="{ width: '64px' }" class="py-3 px-4 text-sm font-semibold text-left "></th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="i in 8" :key="i" class="border-b border-white/10 hover:bg-white/10">
            <td class="py-3 px-4 text-sm text-zinc-300">
              <input class='size-4 border border-white/10 rounded accent-orange-400' type="checkbox" />
            </td>
            <td class="py-3 px-4 text-sm text-zinc-300">1532345</td>
            <td class="py-3 px-4 text-sm text-zinc-300">
              <div class="flex flex-col">
                <span class="font-semibold text-white">Daniel Rubens</span>
                <span>daniel@rubens.com.br</span>
              </div>
            </td>
            <td class="py-3 px-4 text-sm text-zinc-300">7 dias atrás</td>
            <td class="py-3 px-4 text-sm text-zinc-300">3 dias atrás</td>
            <td class="py-3 px-4 text-sm text-zinc-300">
              <button class="bg-black/20 border border-white/10 rounded-md p-1.5">
                <MoreHorizontal class="size-4" />
              </button>
            </td>
          </tr>
        </tbody>
        <tfoot>
          <tr>
            <td class="py-3 px-4 text-sm text-zinc-300" colspan="3">Mostrando 10 de 28 itens</td>
            <td class="py-3 px-4 text-sm text-zinc-300 text-right" colspan="3">
              <div class='inline-flex items-center gap-8'>
                <span>Página 1 de 23</span>
                <div class='flex gap-1.5'>
                  <button class="bg-white/10 border border-white/10 rounded-md p-1.5">
                    <ChevronsLeft class='size-4' />
                  </button>
                  <button class="bg-white/10 border border-white/10 rounded-md p-1.5">
                    <ChevronLeft class='size-4' />
                  </button>
                  <button class="bg-white/10 border border-white/10 rounded-md p-1.5">
                    <ChevronRight class='size-4' />
                  </button>
                  <button class="bg-white/10 border border-white/10 rounded-md p-1.5">
                    <ChevronsRight class='size-4' />
                  </button>
                </div>
              </div>
            </td>
          </tr>
        </tfoot>
      </table>
    </div>
  </div>
</template>

<script setup>
import { Search, MoreHorizontal, ChevronsLeft, ChevronsRight, ChevronLeft, ChevronRight } from 'lucide-vue-next';

</script>
