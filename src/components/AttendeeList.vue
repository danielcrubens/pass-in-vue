<template>
  <div class="flex flex-col gap-4">
    <div class="flex gap-3 items-center">
      <h1 class="text-2xl font-bold">Participantes</h1>
      <div class="px-3 w-72 py-1.5 border border-white  rounded-lg flex items-center gap-3">
        <Search class="size-4 text-emerald-300" />
        <input @input="onSearchInputChanged"
          class="bg-transparent flex-1 outline-none border-0 p-0 text-sm"
          placeholder="Buscar participante..."/>
      </div>
      {{ search }}
    </div>

    <Table>
      <thead>
        <tr class="border-b border-white/70">
          <TableHeader :style="{ width: '48px' }">
            <input class="size-4 bg-black/20 rounded border border-white/10 accent-orange-400" type="checkbox" />
          </TableHeader>
          <TableHeader>Código</TableHeader>
          <TableHeader>Participante</TableHeader>
          <TableHeader>Data de inscrição</TableHeader>
          <TableHeader>Data do check-in</TableHeader>
          <TableHeader :style="{width: '64px'}"></TableHeader>
        </tr>
      </thead>
      <tbody>
        <TableRow v-for="attendee in paginatedAttendees" :key="attendee.id">
          <TableCell>
            <input class='size-4 border border-white/10 rounded accent-orange-400' type="checkbox" />
          </TableCell>
          <TableCell>{{ attendee.id }}</TableCell>
          <TableCell>
            <div class="flex flex-col gap-1">
              <span class="font-semibold text-white">{{ attendee.name }}</span>
              <span>{{ attendee.email }}</span>
            </div>
          </TableCell>
          <TableCell>{{ dayjs().to(attendee.createdAt) }}</TableCell>
          <TableCell>{{ dayjs().to(attendee.checkedInAt) }}</TableCell>
          <TableCell>
            <IconButton transparent class="bg-black/20 border border-white/10 rounded-md p-1.5">
              <MoreHorizontal class="size-4" />
            </IconButton>
          </TableCell>
        </TableRow>
      </tbody>
      <tfoot>
        <tr>
          <TableCell colspan="3">
            Mostrando 10 de {{ attendees.length }} itens
          </TableCell>
          <TableCell class="text-right" colspan="3">
            <div class="inline-flex items-center gap-8">
              <span>
                Página {{ page }} de {{ totalPages }}
              </span>

              <div class="flex gap-1.5">
                <IconButton @click="goToFirstPage" :disabled="page === 1">
                  <ChevronsLeft class="size-4" />
                </IconButton>
                <IconButton @click="goToPreviousPage" :disabled="page === 1">
                  <ChevronLeft class="size-4" />
                </IconButton>
                <IconButton @click="goToNextPage" :disabled="page === totalPages">
                  <ChevronRight class="size-4" />
                </IconButton>
                <IconButton @click="goToLastPage" :disabled="page === totalPages">
                  <ChevronsRight class="size-4" />
                </IconButton>
              </div>
            </div>
          </TableCell>
        </tr>
      </tfoot>
    </Table>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { attendees } from '../data/attendees';
import dayjs from 'dayjs';
import 'dayjs/locale/pt-br';
import relativeTime from 'dayjs/plugin/relativeTime';
dayjs.extend(relativeTime);
dayjs.locale('pt-br');
import Table from './Table/Table.vue';
import TableHeader from './Table/TableHeader.vue';
import TableCell from './Table/TableCell.vue';
import TableRow from './Table/TableRow.vue';
import { Search, MoreHorizontal, ChevronsLeft, ChevronsRight, ChevronLeft, ChevronRight } from 'lucide-vue-next';
import IconButton from './IconButton.vue';

dayjs.extend(relativeTime);
dayjs.locale("pt-br");
const search = ref("");
const page = ref(1);
const pageSize = 10;
const paginatedAttendees = ref([]);

const onSearchInputChanged = (event) => {
  search.value = event.target.value;
};

const updatePaginatedAttendees = () => {
  const startIndex = (page.value - 1) * pageSize;
  const endIndex = Math.min(startIndex + pageSize, attendees.length);
  paginatedAttendees.value = attendees.slice(startIndex, endIndex);
};

const goToFirstPage = () => {
  if (page.value !== 1) {
    page.value = 1;
    updatePaginatedAttendees();
  }
};

const goToLastPage = () => {
  if (page.value !== totalPages.value) {
    page.value = totalPages.value;
    updatePaginatedAttendees();
  }
};

const goToPreviousPage = () => {
  if (page.value > 1) {
    page.value -= 1;
    updatePaginatedAttendees();
  }
};

const goToNextPage = () => {
  if (page.value < totalPages.value) {
    page.value += 1;
    updatePaginatedAttendees();
  }
};

const totalPages = ref(Math.ceil(attendees.length / pageSize));
updatePaginatedAttendees();

</script>
