<template>
  <div class="flex flex-col gap-4">
    <div class="flex gap-3 items-center">
      <h1 class="text-2xl font-bold">Participantes</h1>
      <div class="px-3 w-72 py-1.5 border border-white  rounded-lg flex items-center gap-3">
        <Search class="size-4 text-emerald-300" />
        <input @input="onSearchInputChanged" class="bg-transparent flex-1 outline-none border-0 p-0 text-sm"
          placeholder="Buscar participante..." />
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
          <TableHeader :style="{ width: '64px' }"></TableHeader>
        </tr>
      </thead>
      <tbody>
        <TableRow v-for="attendee in attendees" :key="attendee.id">
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
          <TableCell>
            <span v-if="attendee.checkedInAt === null" class="text-zinc-400">Não fez check-in</span>
            <span v-else>{{ dayjs().to(attendee.checkedInAt) }}</span>
          </TableCell>

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

<script setup lang="ts">
interface Attendee {
  id: string;
  name: string;
  email: string;
  createdAt: string;
  checkedInAt: string;
}
import { onMounted, watch } from 'vue';
import { ref } from 'vue';
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
const attendees = ref<Attendee[]>([]);
const paginatedAttendees = ref([]);
const totalPages = ref(0);

const onSearchInputChanged = (event) => {
  search.value = event.target.value;
};

const updatePaginatedAttendees = () => {
  const startIndex = (page.value - 1) * pageSize;
  const endIndex = Math.min(startIndex + pageSize, attendees.value.length);
  paginatedAttendees.value = attendees.value.slice(startIndex, endIndex);
};

const goToFirstPage = () => {
  if (page.value !== 1) {
    page.value = 1;
  }
};

const goToLastPage = () => {
  if (page.value !== totalPages.value) {
    page.value = totalPages.value;
  }
};

const goToPreviousPage = () => {
  if (page.value > 1) {
    page.value -= 1;
  }
};

const goToNextPage = () => {
  if (page.value < totalPages.value) {
    page.value += 1;
  }
};

const fetchData = async () => {
  try {
    const response = await fetch('http://localhost:3333/events/9e9bd979-9d10-4915-b339-3786b1634f33/attendees');
    const data: { attendees: Attendee[] } = await response.json();
    attendees.value = data.attendees;
    totalPages.value = Math.ceil(attendees.value.length / pageSize);
    updatePaginatedAttendees();
  } catch (error) {
    console.error('Erro ao buscar dados:', error);
  }
};

watch(page, () => {
  updatePaginatedAttendees();
});

onMounted(() => {
  fetchData();
});

</script>
