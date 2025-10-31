<script setup lang="ts">
import { ref } from "vue";
import { onMounted } from "vue";

import HelloWindow from "../components/HelloWindow.vue";
import TabBar from "../components/TabBar.vue";
import ErrorWindow from "../components/ErrorWindow.vue";
import SolfegeQuiz from "./SolfegeQuiz.vue";
import PhonoFanat from "./PhonoFanat.vue";

const showWelcomeWindow = ref(true);

const openWindows = ref<{ id: number; name: string; component: any }[]>([]);
let idCounter = 0;

function closeWelcomeWindow() {
  showWelcomeWindow.value = false;
}

function handleProgramLaunch(component: any, name: string) {
  openWindows.value.push({ id: idCounter++, name, component });
}

function closeProgram(id: number) {
  openWindows.value = openWindows.value.filter((w) => w.id !== id);
}

const trashIcon = new URL(
  "../assets/icons/explorer.exe_14_108.ico",
  import.meta.url,
).href;

const games = [
  {
    icon: new URL("../assets/icons/icons8-music-book-50.png", import.meta.url)
      .href,
    name: "Сольфеджиквиз!",
    component: SolfegeQuiz,
  },
  {
    icon: new URL("../assets/icons/piano_4560961.png", import.meta.url).href,
    name: "ФоноФанат2000",
    component: PhonoFanat, //
  },
];

const windowsXpStartAudio = new URL(
  "../assets/audio/Windows_XP.mp3",
  import.meta.url,
).href;

onMounted(() => {
  const audio = new Audio(windowsXpStartAudio);
  audio.volume = 1;
  audio.play().catch((e) => {
    console.warn(
      " Не удалось воспроизвести звук (возможно, блокировка браузера):",
      e,
    );
  });
});
</script>

<template>
  <div
    class="bg-[url(../assets/background.JPG)] w-screen h-screen bg-cover bg-center overflow-hidden relative"
  >
    <Transition name="fade" mode="out-in">
      <div
        v-if="showWelcomeWindow"
        class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2"
      >
        <HelloWindow @close="closeWelcomeWindow" />
      </div>
    </Transition>

    <component
      v-for="window in openWindows"
      :is="window.component"
      :key="window.id"
      class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2"
      @close="closeProgram(window.id)"
    />

    <div
      v-for="(game, index) in games"
      :key="index"
      class="absolute left-5 flex flex-col items-center cursor-pointer hover:opacity-80 transition"
      :style="{ top: `${index * 90 + 20}px` }"
      @dblclick="() => handleProgramLaunch(game.component, game.name)"
    >
      <img :src="game.icon" alt="game icon" class="w-12 h-12 mb-1" />
      <span class="text-white text-sm font-semibold drop-shadow">{{
        game.name
      }}</span>
    </div>

    <div
      class="absolute bottom-20 right-5 flex flex-col items-center cursor-pointer hover:opacity-80 transition"
      @dblclick="() => handleProgramLaunch(ErrorWindow, 'Корзина')"
    >
      <img :src="trashIcon" alt="trash" class="w-12 h-12 mb-1" />
      <span class="text-white text-sm font-semibold drop-shadow">Корзинак</span>
    </div>

    <!-- панель задач -->
    <div class="absolute bottom-0">
      <TabBar @launch="() => handleProgramLaunch(ErrorWindow, 'Программа')" />
    </div>
  </div>
</template>

<style scoped>
html,
body {
  margin: 0;
  padding: 0;
  overflow: hidden;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
