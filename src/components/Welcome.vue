<script setup>
import { ref } from "vue";
import { motion, AnimatePresence } from "motion-v";
import { Icon } from "@iconify/vue";

const emit = defineEmits(["done"]);

const isExiting = ref(false);
const hasEntered = ref(false);

function handleClick() {
  if (isExiting.value) return;
  isExiting.value = true;
}

function onAnimationComplete() {
  // Ignore the initial mount animation
  if (!hasEntered.value) {
    hasEntered.value = true;
    return;
  }
  // Only emit after the exit animation finishes
  if (isExiting.value) {
    emit("done");
  }
}
</script>

<template>
  <AnimatePresence>
    <motion.div
      v-if="!isExiting"
      key="welcome-overlay"
      class="fixed inset-0 z-50 flex items-center justify-center bg-bg-primary"
      :initial="{ opacity: 0, scale: 1.04, filter: 'blur(16px)' }"
      :animate="{ opacity: 1, scale: 1, filter: 'blur(0px)' }"
      :exit="{
        opacity: 0,
        scale: 1.08,
        filter: 'blur(24px)',
      }"
      :transition="{
        duration: 0.9,
        ease: [0.22, 1, 0.36, 1], // smooth ease-out
      }"
      @animationComplete="onAnimationComplete"
    >
      <motion.button
        type="button"
        class="initicon relative flex items-center justify-center rounded-full p-10 transition-shadow duration-500 hover:shadow-[0_0_60px_rgba(236,72,153,0.25)] focus:outline-none focus:ring-4 focus:ring-pink-deep/20"
        :disabled="isExiting"
        :animate="
          isExiting
            ? {
                scale: [1, 0.85, 1.4, 0],
                y: [0, 8, -40, -120],
                opacity: [1, 1, 0.6, 0],
                rotate: [0, -10, 10, 0],
              }
            : {
                y: [0, -12, 0],
                scale: [1, 1.06, 1],
              }
        "
        :transition="
          isExiting
            ? {
                duration: 1,
                times: [0, 0.15, 0.5, 1],
                ease: 'easeOut',
              }
            : {
                duration: 3,
                repeat: Infinity,
                ease: 'easeInOut',
              }
        "
        @click="handleClick"
      >
        <Icon
          icon="fluent:bow-tie-24-regular"
          class="h-16 w-16 text-pink-deep"
        />
      </motion.button>
    </motion.div>
  </AnimatePresence>
</template>
