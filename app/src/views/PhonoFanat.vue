<script setup lang="ts">
import { ref, onMounted, onUnmounted } from "vue";
import XpWindow from "../components/XpWindow.vue";

const emit = defineEmits(["close"]);

const backgroundMusic = new URL(
  "../assets/audio/game-music-loop-6-144641.mp3",
  import.meta.url,
).href;

const chords = [
  { name: "Малая терция от ми вверх", notes: ["E", "G"] },
  { name: "Чистая прима от до вверх", notes: ["C", "C"] },
  { name: "Большая секунда от ля вверх", notes: ["A", "H"] },
  { name: "Малая секунда от ре вверх", notes: ["D", "D#"] },
  { name: "Чистая октава от до вверх", notes: ["C", "C"] },
  { name: "Большая терция от соль вверх", notes: ["G", "H"] },
  { name: "Большая секста от до вниз", notes: ["C", "F"] },
  { name: "Чистая квинта от ля вниз", notes: ["A", "D"] },
  { name: "Малая терция от соль вниз", notes: ["G", "E"] },
  { name: "Малая секунда от си-бемоль вниз", notes: ["A#", "A"] },
];

const currentChord = ref(chords[Math.floor(Math.random() * chords.length)]);
const selectedNotes = ref<string[]>([]);
const feedback = ref("");
const score = ref(0);
const round = ref(1);
const maxRounds = 10;
const isFinished = ref(false);

function pressNote(note: string) {
  if (selectedNotes.value.length >= 2 || feedback.value) return;

  selectedNotes.value.push(note);

  if (selectedNotes.value.length === 2) {
    const correct = currentChord.value.notes.sort().toString();
    const answer = selectedNotes.value.sort().toString();

    if (correct === answer) {
      feedback.value = "Верно!";
      score.value++;
    } else {
      feedback.value = `Неверно. Правильные ноты: ${currentChord.value.notes.join(" + ")}`;
    }
  }
}

function nextChord() {
  if (isFinished.value) return;

  if (round.value >= maxRounds) {
    isFinished.value = true;
    return;
  }

  selectedNotes.value = [];
  feedback.value = "";
  currentChord.value = chords[Math.floor(Math.random() * chords.length)];
  round.value++;
}

function resetGame() {
  selectedNotes.value = [];
  feedback.value = "";
  score.value = 0;
  round.value = 1;
  isFinished.value = false;
  currentChord.value = chords[Math.floor(Math.random() * chords.length)];
}

const whiteKeys = ["C", "D", "E", "F", "G", "A", "H", "C"];
const blackKeys = [
  { note: "C#", left: "45px" },
  { note: "D#", left: "120px" },
  { note: "F#", left: "265px" },
  { note: "G#", left: "340px" },
  { note: "A#", left: "410px" },
];

const audio = new Audio(backgroundMusic);

let isAudioPlaying = ref(true);

function switchAudio() {
  isAudioPlaying.value = !isAudioPlaying.value;

  if (isAudioPlaying.value) {
    audio.loop = true;
    audio.volume = 1;
    audio.play().catch((e) => {
      console.warn(
        "Не удалось воспроизвести фоновую музыку (возможно, блокировка браузера):",
        e,
      );
      isAudioPlaying.value = false;
    });
  } else {
    audio.pause();
    audio.currentTime = 0;
  }
}

onMounted(() => {
  if (isAudioPlaying.value) {
    audio.loop = true;
    audio.volume = 1;
    audio.play().catch((e) => {
      console.warn(
        "Не удалось воспроизвести фоновую музыку (возможно, блокировка браузера):",
        e,
      );
      isAudioPlaying.value = false;
    });
  } else {
    audio.pause();
    audio.currentTime = 0;
  }
});

onUnmounted(() => {
  audio.pause();
  audio.currentTime = 0;
});
</script>

<template>
  <XpWindow title="ФоноФанат2000 – Семен Семеныч..." @close="$emit('close')">
    <div
      class="w-full h-full bg-gradient-to-br from-emerald-100 via-emerald-400 to-blue-400 p-4 flex flex-col items-center justify-between"
    >
      <div class="w-full flex justify-between items-center text-black px-4">
        <span class="font-bold text-lg"
          >Раунд: {{ Math.min(round, maxRounds) }} / {{ maxRounds }}</span
        >
        <span class="font-bold text-lg">Очки: {{ score }}</span>
        <button @click="switchAudio" class="cursor-pointer">
          <img
            class="h-10 w-10"
            v-if="isAudioPlaying"
            src="../assets/icons/medium-volume.png"
          />
          <img
            class="h-10 w-10"
            v-if="!isAudioPlaying"
            src="../assets/icons/mute.png"
          />
        </button>
      </div>

      <div class="text-black text-2xl font-bold mt-4" v-if="!isFinished">
        Интервал: {{ currentChord.name }}
      </div>

      <div class="text-black text-lg font-semibold mt-2 text-center">
        <p>{{ feedback }}</p>
        <button
          v-if="feedback && !isFinished"
          @click="nextChord"
          class="px-4 py-1 mt-10 bg-blue-600 text-white rounded-lg hover:bg-blue-700 transition cursor-pointer"
        >
          Далее
        </button>
      </div>

      <div v-if="isFinished" class="text-black text-center mt-6">
        <h2 class="text-2xl font-bold mb-2">Игра завершена!</h2>
        <p class="text-lg mb-4">Ты набрал(а): {{ score }} из {{ maxRounds }}</p>
        <button
          @click="resetGame"
          class="px-4 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700"
        >
          Рестарт?!
        </button>
      </div>

      <div
        class="relative mt-8 w-full max-w-4xl h-48 select-none rounded-xl shadow-xl overflow-hidden items-center bg-gray-100"
      >
        <div class="flex h-full">
          <div
            v-for="note in whiteKeys"
            :key="note"
            @click="pressNote(note)"
            class="w-[80px] h-full border-r-2 border-black bg-white active:bg-gray-200 cursor-pointer hover:bg-gray-100 flex items-end justify-center relative z-10"
            :class="{ 'bg-green-700': selectedNotes.includes(note) }"
          >
            <span class="mb-2 text-sm">{{ note }}</span>
          </div>
        </div>

        <div
          v-for="key in blackKeys"
          :key="key.note"
          @click="pressNote(key.note)"
          :style="{ left: key.left }"
          class="absolute top-0 w-[55px] h-[60%] bg-black active:bg-gray-700 z-20 rounded-lg cursor-pointer hover:bg-gray-800 flex items-end justify-center"
          :class="{ 'bg-green-700': selectedNotes.includes(key.note) }"
        ></div>
      </div>
    </div>
  </XpWindow>
</template>
