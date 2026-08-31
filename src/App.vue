<script setup>
import { ref, onMounted } from "vue";
import { motion } from "motion-v";
import Welcome from "#/Welcome.vue";
import Page1 from "#/Page1.vue";
import Page2 from "#/Page2.vue";
import Page3 from "#/Page3.vue";
import Page4 from "#/Page4.vue";
import Page5 from "#/Page5.vue";

const showWelcome = ref(true);
const audioRef = ref(null);
const needsInteraction = ref(false);

async function tryAutoplay() {
  try {
    if (!audioRef.value) return;
    await audioRef.value.play();
    needsInteraction.value = false;
  } catch {
    // Browser blocked autoplay
    needsInteraction.value = true;
  }
}

function startPlayback() {
  if (!audioRef.value) return;
  audioRef.value.play().catch((err) => console.log("Playback error:", err));
  needsInteraction.value = false;
}

function handleDone() {
  showWelcome.value = false;
  // User interaction occurred by dismissing Welcome screen, so audio can safely start
  startPlayback();
}

onMounted(() => {
  tryAutoplay();
});
</script>

<template>
  <div class="relative min-h-dvh overflow-hidden bg-bg-primary">
    <!-- Global Audio Element -->
    <audio ref="audioRef" src="/song.mp3" loop></audio>

    <Welcome v-if="showWelcome" @done="handleDone" />

    <motion.div
      v-else
      class="relative z-10"
      :initial="{ opacity: 0, y: 30, filter: 'blur(12px)' }"
      :animate="{ opacity: 1, y: 0, filter: 'blur(0px)' }"
      :transition="{
        duration: 0.8,
        ease: [0.22, 1, 0.36, 1],
        delay: 0.15,
      }"
    >
      <Page5 />
    </motion.div>
  </div>
</template>
