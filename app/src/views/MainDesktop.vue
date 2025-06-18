
<script setup lang="ts">
import { ref } from 'vue';
import HelloWindow from '../components/HelloWindow.vue';
import TabBar from '../components/TabBar.vue';

const showWelcomeWindow = ref(true);

function closeWelcomeWindow() {
  showWelcomeWindow.value = false;
};

</script>

<template>
  <div class="bg-[url(../assets/background.JPG)] w-screen h-screen bg-cover bg-center overflow-hidden">
    <Transition name="fade" mode="out-in">
      <div 
      v-if="showWelcomeWindow"
      class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2"
      >
        <HelloWindow @close="closeWelcomeWindow" />
      </div>
    </Transition>
      <div class="absolute">
        <TabBar />
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

/* Анимация через Transition */
.fade-enter-active, .fade-leave-active {
  transition: opacity 0.5s ease;
}
.fade-enter-from, .fade-leave-to {
  opacity: 0;
}
</style>
