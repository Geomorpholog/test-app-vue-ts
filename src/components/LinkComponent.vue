<script setup lang="ts">
  import { RouterLink } from 'vue-router'
  const props = defineProps<{
    src:string
    alt:string
    to:string
    name:string
  }>()
</script>

<template>
   <RouterLink class="v-[500px] mb-[8px] flex h-[68px] text-lg text-sky-950 grow items-center justify-center gap-2  bg-sky-200 p-3 text-sm font-medium hover:bg-gray-700 hover:text-amber-100 md:flex-none md:justify-start md:p-2 md:px-3 last:mb-0" 
              :to="props.to"
              activeClass="bg-slate-900 text-white">
              <img :src="props.src" :alt="props.alt" width="40px" height="40px"/>{{ props.name }}</RouterLink>
</template>