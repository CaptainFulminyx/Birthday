<script setup>
import { ref } from "vue";
import { motion } from "motion-v";
import Welcome from "./components/Welcome.vue";
import Page1 from "./components/Page1.vue";
import Page2 from "./components/Page2.vue";
import Page3 from "./components/Page3.vue";
import Page4 from "./components/Page4.vue";
import Page5 from "./components/Page5.vue";

const showWelcome = ref(true);
</script>

<template>
  <div class="relative min-h-dvh overflow-hidden bg-bg-primary">
    <Welcome v-if="showWelcome" @done="showWelcome = false" />
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
      <Page1 />
      <Page2 />
      <Page3 />
      <Page4 />
      <Page5 />
    </motion.div>
  </div>
</template>
