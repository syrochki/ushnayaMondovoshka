<script setup lang="ts">
import { onMounted } from "vue";

const emit = defineEmits(["close"]);

function handleClose() {
  emit("close");
}

const errorSound = new URL("../assets/audio/erro.mp3", import.meta.url).href;

onMounted(() => {
  const audio = new Audio(errorSound);
  audio.volume = 1;
  audio.play().catch((e) => {
    console.warn(
      "Не удалось воспроизвести звук (возможно, блокировка браузера):",
      e,
    );
  });
});
</script>

<template>
  <div
    class="w-96 p-6 bg-white border border-gray-300 rounded-lg shadow-lg flex flex-col items-center"
  >
    <h2 class="text-xl font-bold mb-2 text-center">
      Приложение на данный момент не доступно:(
    </h2>
    <p class="text-gray-700 mb-4 text-center">
      Возможно, когда нибудь починим...
    </p>
    <button
      class="px-4 py-2 bg-red-700 text-white rounded-lg hover:bg-red-600 hover:shadow-lg transition-colors duration-200"
      @click="handleClose"
    >
      Закрыть
    </button>
  </div>
</template>
