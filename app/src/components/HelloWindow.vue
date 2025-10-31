<script setup lang="ts">
const emit = defineEmits(["close"]);

function handleClose() {
  emit("close");
}
</script>

<template>
  <div
    class="w-96 p-6 bg-white border border-gray-300 rounded-lg shadow-lg flex flex-col items-center"
  >
    <h2 class="text-xl font-bold mb-2 text-center">
      Вас приветстует MusicXP<br />
      Добро пожаловать!
    </h2>
    <p class="text-gray-700 mb-4 text-center">
      Это операционная система-тренажёр по музыке в стиле 00-х.<br />
      Для лучшего опыта, разрешите веб-приложению доступ к аудио.<br />
    </p>
    <button
      class="px-4 py-2 bg-blue-900 text-white rounded hover:bg-blue-700 hover:shadow-lg transition-colors duration-200 cursor-pointer"
      @click="handleClose"
    >
      OK
    </button>
  </div>
</template>
