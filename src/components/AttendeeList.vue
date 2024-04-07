<template>
  <div class="flex flex-col gap-4">
    <div class="flex gap-3 items-center">
      <h1 class="text-2xl font-bold">Participantes</h1>
      <div class="px-3 w-72 py-1.5 border border-white/10  rounded-lg flex items-center gap-3">
        <Search class="size-4 text-emerald-300" />
        <input v-model="search" @input="onSearchInputChanged" class="bg-transparent flex-1 outline-none border-0 p-0 text-sm"
          placeholder="Buscar participante..." />
      </div>
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
            Mostrando {{ attendees.length }} de {{ total }} itens
          </TableCell>
          <TableCell class="text-right" colspan="3">
            <div class="inline-flex items-center gap-8">
              <span>
                Página {{ page }} de {{ total }}
              </span>

              <div class="flex gap-1.5">
                <IconButton @click="goToFirstPage" :disabled="page === 1">
                  <ChevronsLeft class="size-4" />
                </IconButton>
                <IconButton @click="goToPreviousPage" :disabled="page === 1">
                  <ChevronLeft class="size-4" />
                </IconButton>
                <IconButton @click="goToNextPage" :disabled="page === total">
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
import { ref, watch, onMounted  } from 'vue';
import dayjs from 'dayjs';
import 'dayjs/locale/pt-br';
import relativeTime from 'dayjs/plugin/relativeTime';
import Table from './Table/Table.vue';
import TableHeader from './Table/TableHeader.vue';
import TableCell from './Table/TableCell.vue';
import TableRow from './Table/TableRow.vue';
import { Search, MoreHorizontal, ChevronsLeft, ChevronsRight, ChevronLeft, ChevronRight } from 'lucide-vue-next';
import IconButton from './IconButton.vue';

dayjs.extend(relativeTime);
dayjs.locale("pt-br");

interface Attendee {
  id: string;
  name: string;
  email: string;
  createdAt: string;
  checkedInAt: string | null;
}

const search = ref('');
const page = ref(1);
const total = ref(0);
const attendees = ref<Attendee[]>([]);
const BASE_URL = import.meta.env.VITE_API_BASE_URL;


const fetchData = () => {
  const url = new URL(`${BASE_URL}/events/4d581edb-3f8c-4224-9182-c4398cfea080/attendees`);
  const params = new URLSearchParams({
    pageIndex: String(page.value - 1),
    query: search.value
  });
  url.search = params.toString();
 
  fetch(url)
    .then(response => response.json())
    .then(data => {
      attendees.value = data.attendees;
      total.value = data.total;
    });
};

const onSearchInputChanged = (event) => {
  search.value = event.target.value;
  if (search.value.trim() === '') { // Verifica se o campo de pesquisa está vazio
    removeSearchParameterFromURL();
  } else {
    setCurrentPage(1);
  }
};

const removeSearchParameterFromURL = () => {
  const url = new URL(window.location.toString());
  url.searchParams.delete('search'); // Remove o parâmetro 'search' da URL
  url.search = url.searchParams.toString();
  window.history.pushState({}, '', url);
};

const setCurrentPage = (pageNumber: number) => {
  const url = new URL(window.location.toString());
  url.searchParams.set('page', String(pageNumber));
  if (search.value.trim() !== '') { // Verifica se há uma pesquisa ativa
    url.searchParams.set('search', search.value);
  } else { // Remove o parâmetro 'search' se não houver pesquisa ativa
    url.searchParams.delete('search');
  }
  url.search = url.searchParams.toString();
  window.history.pushState({}, '', url);
  page.value = pageNumber;
};

const goToFirstPage = () => setCurrentPage(1);
const goToLastPage = () => setCurrentPage(Math.ceil(total.value / 10));
const goToPreviousPage = () => setCurrentPage(page.value - 1);
const goToNextPage = () => setCurrentPage(page.value + 1);

// Carregar dados na montagem do componente e sempre que a página mudar
fetchData();
watch(page, fetchData);
watch(search, fetchData);

const initializePageFromURL = () => {
  const url = new URL(window.location.toString());
  const pageParam = url.searchParams.get('page');
  if (pageParam) {
    const pageNumber = parseInt(pageParam);
    if (!isNaN(pageNumber)) {
      page.value = pageNumber; 
    }
  }
};

onMounted(initializePageFromURL);
</script>