<script setup lang="ts">
import { ref } from "vue";

const emit = defineEmits(["launch"]);

const showMenu = ref(false);

const startIcon = new URL("../assets/icons/windows_15526.ico", import.meta.url)
  .href;
const programs = [
  {
    icon: new URL("../assets/icons/explorer.exe_14_100.ico", import.meta.url)
      .href,
    name: "Проводник",
  },
  {
    icon: new URL("../assets/icons/IEXPLORE.EXE_14_32528.ico", import.meta.url)
      .href,
    name: 'Самый "быстрый" браузер',
  },
  {
    icon: new URL("../assets/icons/explorer.exe_14_254.ico", import.meta.url)
      .href,
    name: "Почта",
  },
];

function toggleMenu() {
  showMenu.value = !showMenu.value;
}

function click() {
  emit("launch");
  showMenu.value = false;
}
</script>

<template>
  <nav
    class="w-full h-10 fixed bottom-0 bg-gradient-to-t from-[#1139a8] to-[#4b8ed5] flex items-center px-2 shadow-inner border-t border-blue-900 z-50"
  >
    <button
      class="flex items-center gap-2 px-7 py-1 mx-1 pr-6 rounded-r-full border border-[#0a5e0a] bg-gradient-to-b from-[#3cb043] to-[#1e5d1e] text-white font-semibold shadow-inner hover:brightness-110 active:translate-y-px active:brightness-90 transition absolute -left-2 bottom-0 cursor-pointer"
      @click="toggleMenu"
    >
      <img :src="startIcon" alt="start" class="w-8 h-8" />
      <span class="drop-shadow-sm">пупс</span>
    </button>

    <div
      v-if="showMenu"
      class="absolute bottom-10 left-0 w-56 bg-[#d4d0c8] border border-gray-700 shadow-xl text-black font-normal z-40 rounded-tr-xl"
    >
      <div
        v-for="(item, index) in programs"
        :key="index"
        class="flex items-center px-2 py-2 hover:bg-blue-600 hover:text-white cursor-pointer transition"
        @click="click"
      >
        <img :src="item.icon" class="w-6 h-6 mr-2" />
        <span>{{ item.name }}</span>
      </div>
    </div>

    <div class="flex items-center pl-36 h-full">
      <div
        v-for="(item, index) in programs"
        :key="index"
        class="flex items-center mx-1 rounded hover:bg-blue-800 transition cursor-pointer"
        @click="click"
      >
        <img :src="item.icon" alt="icon" class="w-7 h-7 mx-3 my-1" />
      </div>
    </div>
  </nav>
</template>

<style scoped>
nav {
  font-family: "Tahoma", sans-serif;
  font-size: 16px;
}
</style>
