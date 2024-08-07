<script setup lang="ts">
import homeIcon from '@/assets/icons/Home.svg';
import usersIcon from '@/assets/icons/Users.svg'
import LinkComponent from './LinkComponent.vue';
const linkArr = [
  {
    name:"Home",
    to:"/",
    src:homeIcon,
    alt:"Home icon"
  },
  {
    name:"Users",
    to:"/Users",
    src:usersIcon,
    alt:"Users icon"
  }
]
</script>

<template>
    <div className="bg-sky-200 w-[500px] h-screen inline flex flex-col fixed">
        <header class="h-full bg-sky-300 border-8 border-sky-300">
          
          <img alt="Vue logo" class="logo mb-[25px]" src="@/assets/images/Logo.png"  />

          <div class="wrapper">
            <nav>
                <LinkComponent v-for="link in linkArr" :key="link.name" :name="link.name" :alt="link.alt" :src="link.src" :to="link.to"/>
            </nav>
          </div>
        </header>
      </div>

</template>