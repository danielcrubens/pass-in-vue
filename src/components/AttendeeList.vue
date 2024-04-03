<template>
  <div class="flex flex-col gap-10">
    <div class="flex gap-3 items-center">
      <h1 class="text-2xl font-bold">Participantes</h1>
      <div class="px-3 w-72 py-1.5 border border-white/10 b rounded-lg text-sm flex items-center gap-3">
        <Search class="text-emerald-300" :size="16" />
        <input @input="onSearchInputChange"
          class="bg-transparent flex-1 outline-none border-0 p-0 text-sm ring-0 accent-orange-400"
          placeholder="Buscar participante..." />
      </div>
      {{ setSearch }}
    </div>
    <Table>
      <thead>
        <tr class="border-b border-white/10">
          <TableHeader :style="{ width: '48px' }">
            <input class="size-4 bg-black/20 rounded border border-white/10 accent-orange-400" type="checkbox" />
          </TableHeader>
          <TableHeader>Código</TableHeader>
          <TableHeader>Participante</TableHeader>
          <TableHeader>Data de inscrição</TableHeader>
          <TableHeader>Data do check-in</TableHeader>
          <TableHeader :style="{ width: '64px' }"></TableHeader>
        </tr>
      </thead>
      <tbody>
        <TableRow v-for="i in 8" :key="i">
          <TableCell>
            <input class='size-4 border border-white/10 rounded accent-orange-400' type="checkbox" />
          </TableCell>
          <TableCell>1532345
          </TableCell>
          <TableCell>
            <div class="flex flex-col">
              <span class="font-semibold text-white">Daniel Rubens</span>
              <span>daniel@rubens.com.br</span>
            </div>
          </TableCell>
          <TableCell>7 dias atrás</TableCell>
          <TableCell>3 dias atrás</TableCell>
          <TableCell>
            <IconButton :transparent="BgTransparent">
              <MoreHorizontal class="size-4" />
            </IconButton>
          </TableCell>
        </TableRow>
      </tbody>
      <tfoot>
        <tr>
          <TableCell colspan="3">Mostrando 10 de 28 itens</TableCell>
          <td class="py-3 px-4 text-sm text-zinc-300 text-right" colspan="3">
            <div class='inline-flex items-center gap-8'>
              <span>Página 1 de 23</span>
              <div class='flex gap-1.5'>
                <IconButton>
                  <ChevronsLeft class='size-4' />
                </IconButton>
                <IconButton>
                  <ChevronLeft class='size-4' />
                </IconButton>
                <IconButton>
                  <ChevronRight class='size-4' />
                </IconButton>
                <IconButton>
                  <ChevronsRight class='size-4' />
                </IconButton>
              </div>
            </div>
          </td>
        </tr>
      </tfoot>
    </Table>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import Table from './Table/Table.vue';
import TableHeader from './Table/TableHeader.vue';
import TableCell from './Table/TableCell.vue';
import TableRow from './Table/TableRow.vue';
import { Search, MoreHorizontal, ChevronsLeft, ChevronsRight, ChevronLeft, ChevronRight } from 'lucide-vue-next';
import IconButton from './IconButton.vue';
const BgTransparent = ref(true);

const setSearch = ref('');
function onSearchInputChange(event) {
  setSearch.value = event.target.value;

}
</script>
