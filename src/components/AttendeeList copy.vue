<template>
  <div class="flex flex-col gap-4">
    <div class="flex gap-3 items-center">
      <h1 class="text-2xl font-bold">Participantes</h1>
      <div class="px-3 w-72 py-1.5 border border-white/10 rounded-lg text-sm flex items-center gap-3">
        <SearchIcon class='size-4 text-emerald-300'/>
        <input class="bg-transparent flex-1 outline-none" type="text" placeholder="Buscar participantes..." />
      </div>
    </div>

    <div class="border border-white/10 rounded-lg">
      <table class="w-full">
        <thead>
          <tr class="border-b border-white/10">
            <th class="py-3 px-4 text-sm font-semibold text-left w-11">
              <input class='size-4 border border-white/10 rounded accent-orange-400' type="checkbox" />
            </th>
            <th class="py-3 px-4 text-sm font-semibold text-left">Código</th>
            <th class="py-3 px-4 text-sm font-semibold text-left">Participante</th>
            <th class="py-3 px-4 text-sm font-semibold text-left">Data de inscrição</th>
            <th class="py-3 px-4 text-sm font-semibold text-left">Data do check-in</th>
            <th class="py-3 px-4 text-sm font-semibold text-left w-16"></th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(_, i) in 8" :key="i" class="border-b border-white/10">
            <td class="py-3 px-4 text-sm text-zinc-300">
              <input class='size-4 border border-white/10 rounded accent-orange-400' type="checkbox" />
            </td>
            <td class="py-3 px-4 text-sm text-zinc-300">1532345</td>
            <td class="py-3 px-4 text-sm text-zinc-300">
              <div class="flex flex-col">
                <span class="font-semibold text-white">Rômulo Bastos</span>
                <span>ola@romulobastos.com.br</span>
              </div>
            </td>
            <td class="py-3 px-4 text-sm text-zinc-300">7 dias atrás</td>
            <td class="py-3 px-4 text-sm text-zinc-300">3 dias atrás</td>
            <td class="py-3 px-4 text-sm text-zinc-300">
              <button class="bg-black/20 border border-white/10 rounded-md p-1.5">
                <MoreHorizontalIcon class="size-4" />
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
                    <ChevronsLeftIcon class='size-4'/>
                  </button>
                  <button class="bg-white/10 border border-white/10 rounded-md p-1.5">
                    <ChevronLeftIcon class='size-4'/>
                  </button>
                  <button class="bg-white/10 border border-white/10 rounded-md p-1.5">
                    <ChevronRightIcon class='size-4'/>
                  </button>
                  <button class="bg-white/10 border border-white/10 rounded-md p-1.5">
                    <ChevronsRightIcon class='size-4'/>
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

<script>

export default {
  name: 'AttendeeList'
}
</script>
