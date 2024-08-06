<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'

</script>

<template>
<div className="bg-sky-200 w-[500px] h-screen inline flex flex-col fixed">
  <header class="h-full bg-sky-300 border-8 border-sky-300">
    
    <img alt="Vue logo" class="logo mb-[25px]" src="@/assets/images/Logo.png"  />

    <div class="wrapper">
      <nav>
        <RouterLink class="v-[500px] mb-[8px] flex h-[68px] text-lg text-sky-950 grow items-center justify-center gap-2  bg-sky-200 p-3 text-sm font-medium hover:bg-gray-700 hover:text-amber-100 md:flex-none md:justify-start md:p-2 md:px-3 last:mb-0" 
        to="/"
        activeClass="bg-slate-900 text-white">Home</RouterLink>
        <RouterLink class=" mb-[8px] flex h-[68px] text-lg text-sky-950 grow items-center justify-center gap-2  bg-sky-200 p-3 text-sm font-medium hover:bg-gray-700 hover:text-amber-100 md:flex-none md:justify-start md:p-2 md:px-3 last:mb-0" 
        to="/Users"
        activeClass="bg-slate-900 text-white">Users</RouterLink>
      </nav>
    </div>
  </header>
</div>
  

  <RouterView />
</template>

<style scoped>

</style>
