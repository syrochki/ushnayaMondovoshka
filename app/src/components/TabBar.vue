<script setup lang="ts">

const startIcon = new URL('../assets/icons/windows_15526.ico', import.meta.url).href
const programs = [
  { icon: new URL('../assets/icons/explorer.exe_14_100.ico', import.meta.url).href },
  { icon: new URL('../assets/icons/IEXPLORE.EXE_14_32528.ico', import.meta.url).href },
  { icon: new URL('../assets/icons/explorer.exe_14_254.ico', import.meta.url).href },
  { icon: new URL('../assets/icons/explorer.exe_14_108.ico', import.meta.url).href },
]

</script>

<template>
  <nav class="w-full h-12 fixed bottom-0 bg-[#0a246a] flex items-center px-2 shadow-inner border-t border-blue-900 z-50">
    <button
      class="flex items-center gap-2 px-7 py-1 mx-1 pr-6 
      rounded-lg border border-[#0a5e0a] bg-gradient-to-b from-[#3cb043] to-[#1e5d1e] text-white font-semibold shadow-inner 
      hover:brightness-110 active:translate-y-px active:brightness-90 transition absolute -left-2 bottom-0"
    >
      <img :src="startIcon" alt="start" class="w-10 h-10" />
      <span class="drop-shadow-sm">пупс</span>
    </button>
    <div class="flex items-center pl-36 h-full">
        <div 
        v-for="(item, index) in programs"
        :key="index"
        class="flex items-center mx-1 rounded hover:bg-blue-800 transition cursor-pointer"
        > 
        <img :src="item.icon" alt="icon" class="w-8 h-8 mx-3 my-1" />
        </div>
    </div>
  </nav>
</template>

<style scoped>
nav {
  font-family: 'Tahoma', sans-serif;
  font-size: 16px;
}
</style>
