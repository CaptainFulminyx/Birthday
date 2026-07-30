<script setup>
import { ref } from "vue";
import { motion, AnimatePresence } from "motion-v";
import { Icon } from "@iconify/vue";

// Controls whether the intro layer has finished its exit and
// the slotted content underneath is now shown.
const revealed = ref(false);

// Prevents re-triggering the animation on a second click,
// and tells the shake/fall keyframes to play.
const isShaking = ref(false);

function handleClick() {
  if (isShaking.value) return;
  isShaking.value = true;
}

// motion-v fires @animationComplete for ANY animate change,
// including the very first render — the isShaking guard makes
// sure we only reveal once the shake+fall sequence actually ran.
function handleComplete() {
  if (isShaking.value) {
    revealed.value = true;
  }
}
</script>

<template>
  <div class="relative min-h-dvh overflow-hidden bg-bg-primary">
    <!-- Real app content, mounted only once the intro is gone -->
    <div v-if="revealed" class="relative z-0">
      <slot />
    </div>

    <AnimatePresence>
      <motion.div
        v-if="!revealed"
        class="fixed inset-0 z-50 flex items-center justify-center bg-bg-primary"
        :animate="
          isShaking
            ? {
                rotate: [],
                y: [0, 0, 0, 0, 0, 0, 0, 0, 20, 220, 520],
                opacity: [1, 1, 1, 1, 1, 1, 1, 1, 1, 0.7, 0],
              }
            : { rotate: 0, y: 0, opacity: 1 }
        "
        :transition="{
          duration: 1.4,
          times: [0, 0.05, 0.1, 0.15, 0.2, 0.25, 0.3, 0.35, 0.5, 0.75, 1],
          ease: 'easeInOut',
        }"
        @animationComplete="handleComplete"
      >
        <button
          type="button"
          class="flex h-28 w-28 items-center justify-center rounded-full bg-cream shadow-glow transition-transform active:scale-95 disabled:cursor-default"
          :disabled="isShaking"
          aria-label="Open birthday surprise"
          @click="handleClick"
        >
          <Icon
            icon="fluent:bow-tie-24-regular"
            class="h-16 w-16 text-pink-deep"
          />
        </button>
      </motion.div>
    </AnimatePresence>
  </div>
</template>
