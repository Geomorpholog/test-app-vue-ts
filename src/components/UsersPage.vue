<script setup lang="ts">
import TablePage from './TablePage.vue'
import { } from "@tanstack/vue-table";
import type { ColumnDef } from '@tanstack/vue-table';
import { ref, onMounted } from 'vue'

export type User = {
  id: number;
  firstName: string;
  lastName: string;
  age: number;
  gender: string;
  eyeColor: string;
};


type Data = {
  users: User[]
};

const getData = async (): Promise<Data | undefined> => {
  const res = await fetch('https://dummyjson.com/users')
  if (!res.ok) {
    throw new Error('Failed to fetch data')
  }
  return res.json()
}
const fetchUsers = async (): Promise<User[]> => {
  try {
    const data: Data | undefined = await getData();
    if (data) {

      return data.users;

    } else {
      console.error("Failed to fetch data, array is empty");
      return [];
    }
  } catch (error) {
    console.error("Failed to fetch data:", error);
    return [];
  }
};

const columns: ColumnDef<User, string|number>[] = [
  {
    accessorKey: 'id',     
    header: 'ID',          
    cell: info => info.getValue(), 
  },
  {
    accessorKey: 'firstName',
    header: 'First Name',
    cell: info => info.getValue(),
  },
  {
    accessorKey: 'lastName',
    header: 'Last Name',
    cell: info => info.getValue(),
  },
  {
    accessorKey: 'age',
    header: 'Age',
    cell: info => info.getValue(),
    sortingFn: 'alphanumeric',
    enableSorting: true,
  },
  {
    accessorKey: 'gender',
    header: 'Gender',
    cell: info => info.getValue(),
  },
  {
    accessorKey: 'eyeColor',
    header: 'Eye Color',
    cell: info => info.getValue(),
  }
];

const loading = ref(true);
const data = ref<User[]>([]);
onMounted(async () => {
  try {
    loading.value = true;
    data.value = await fetchUsers();
  } finally {
    loading.value = false;
  }
});


</script>

<template>
  <div v-if="loading" :class="['w-4/4', 'h-screen', 'flex', 'justify-center', 'items-center']">
    <p :class="['text-5xl', 'animate-bounce']">Loading Data...</p>
  </div>
  <div v-else class="w-full  min-h-screen bg-sky-100 inline-block">
    <TablePage  :columns :data /> 
  </div>
  

</template>
