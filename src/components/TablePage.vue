<script setup lang="ts">
import { FlexRender, getCoreRowModel, getPaginationRowModel, useVueTable, getSortedRowModel } from '@tanstack/vue-table'
import { ref } from 'vue';
import type { ColumnDef, SortingState } from '@tanstack/vue-table';


const props = defineProps<{
    columns: ColumnDef<any, any>[];
    data: object[];
}>();

const INITIAL_PAGE_INDEX = 0
const goToPageNumber = ref(INITIAL_PAGE_INDEX + 1)
const pageSizes = [5, 10, 20, 30]
const sorting = ref<SortingState>([])


const table = useVueTable({
    data: props.data,
    columns: props.columns,
    getCoreRowModel: getCoreRowModel(),
    state: {
        get sorting() {
            return sorting.value
        },
    },
    onSortingChange: updaterOrValue => {
        sorting.value =
            typeof updaterOrValue === 'function'
                ? updaterOrValue(sorting.value)
                : updaterOrValue
    },
    getSortedRowModel: getSortedRowModel(),
    getPaginationRowModel: getPaginationRowModel(),
});


function handleGoToPage(e: any) {
    const page = e.target.value ? Number(e.target.value) - 1 : 0
    goToPageNumber.value = page + 1
    table.setPageIndex(page)
}

function handlePageSizeChange(e: any) {
    table.setPageSize(Number(e.target.value))
}


</script>

<template>
    <div class="w-[50px] h-[200px] flex flex-col justify-between items-center fixed top-[250px] left-[520px]">
        <p className="text-center">First page</p>
        <button class="w-[50px] h-[50px] bg-sky-300 text-xl font-bold border-4 border-sky-950 hover:bg-gray-700"
            @click="() => table.setPageIndex(0)" :disabled="!table.getCanPreviousPage()">
            «
        </button>
        <p className="text-center">Prev page</p>
        <button class="w-[50px] h-[50px] bg-sky-300 text-xl font-bold border-4 border-sky-950 hover:bg-gray-700"
            @click="() => table.previousPage()" :disabled="!table.getCanPreviousPage()">
            ‹
        </button>
    </div>
    <div class="w-[50px] h-[200px] flex flex-col justify-between items-center fixed top-[250px] right-[20px]">
        <p className="text-center">Last page</p>
        <button class="w-[50px] h-[50px] bg-sky-300 text-xl font-bold border-4 border-sky-950 hover:bg-gray-700"
            @click="() => table.setPageIndex(table.getPageCount() - 1)" :disabled="!table.getCanNextPage()">
            »
        </button>
        <p className="text-center">Next page</p>
        <button class="w-[50px] h-[50px] bg-sky-300 text-xl font-bold border-4 border-sky-950 hover:bg-gray-700"
            @click="() => table.nextPage()" :disabled="!table.getCanNextPage()">
            ›
        </button>

    </div>
    <div class="w-[50px] h-[200px] fixed flex flex-col left-[520px]">

        <div>Page</div>
        <strong>
            {{ table.getState().pagination.pageIndex + 1 }} of
            {{ table.getPageCount() }}
        </strong>

        <p className="flex items-center gap-1">Go to page:</p>
        <input min=1 :max="table.getPageCount()" type="number" :value="goToPageNumber" @change="handleGoToPage"
            class="w-12 text-2xl" />
        <p class="flex items-center gap-1">
            Show rows:</p>
        <select class="w-12 text-2xl" :value="table.getState().pagination.pageSize" @change="handlePageSizeChange">
            <option :key="pageSize" :value="pageSize" v-for="pageSize in pageSizes">
                {{ pageSize }}
            </option>
        </select>
    </div>




    <div :class="['w-full', 'h-full', 'flex', 'justify-center', 'items-center', 'flex-col']">
        <table :class="['text-3xl', 'text-sky-950', 'border-collapse:collapse']">
            <thead>
                <tr class="h-[100px]" v-for="headerGroup in table.getHeaderGroups()" :key="headerGroup.id">
                    <th class="text-start border-8 border-sky-950 px-10 bg-sky-300"
                        :class="header.column.getCanSort() ? 'cursor-pointer select-none' : ''"
                        @click="header.column.getToggleSortingHandler()?.($event)" v-for="header in headerGroup.headers"
                        :key="header.id" :colSpan="header.colSpan">
                        <template v-if="!header.isPlaceholder">
                            <FlexRender :render="header.column.columnDef.header" :props="header.getContext()" />

                            {{
                                { asc: ' 🔼', desc: ' 🔽' }[
                                header.column.getIsSorted() as string
                            ]
                            }}
                        </template>
                    </th>
                </tr>
            </thead>
            <tbody>
                <tr class="h-[100px]" v-for="row in table.getRowModel().rows" :key="row.id">
                    <td class="border-8 border-sky-950 pl-8" v-for="cell in row.getVisibleCells()" :key="cell.id">
                        <FlexRender :render="cell.column.columnDef.cell" :props="cell.getContext()" />
                    </td>
                </tr>
            </tbody>
        </table>
    </div>


</template>