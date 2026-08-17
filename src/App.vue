<script setup>
import { ref } from "vue";
import { motion } from "motion-v";
import Welcome from "./components/Welcome.vue";
import Page from "./components/Page1.vue";

const showWelcome = ref(false); // FOR DEVELOPMENT ONLY!!!!!!!!!!;
</script>

<template>
  <div class="relative min-h-dvh overflow-hidden bg-bg-primary">
    <!-- Welcome handles its own exit animation, then emits 'done' -->
    <Welcome v-if="showWelcome" @done="showWelcome = false" />

    <!-- Page enters with a smooth upward fade only after Welcome is gone -->
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
      <Page />
    </motion.div>
  </div>
</template>
