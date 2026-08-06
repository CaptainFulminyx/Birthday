<!-- Welcome.vue -->
<script setup>
import { ref } from "vue";
import { motion, AnimatePresence } from "motion-v";
import { Icon } from "@iconify/vue";

const emit = defineEmits(["done"]);

const HOLD_DURATION = 1200; // ms to hold before dismissing
const RADIUS = 70;
const CIRCUMFERENCE = 2 * Math.PI * RADIUS;

const isExiting = ref(false);
const isHolding = ref(false);
const ringOffset = ref(CIRCUMFERENCE); // 0 = full ring visible, CIRCUMFERENCE = fully shrunk

let holdTimeoutId = null;

function startHold(e) {
  if (isExiting.value) return;
  e.preventDefault();
  isHolding.value = true;
  ringOffset.value = 0; // triggers shrink transition over HOLD_DURATION

  holdTimeoutId = setTimeout(() => {
    isHolding.value = false;
    isExiting.value = true; // -> v-if unmounts -> AnimatePresence plays :exit
  }, HOLD_DURATION);
}

function cancelHold() {
  if (holdTimeoutId) {
    clearTimeout(holdTimeoutId);
    holdTimeoutId = null;
  }
  if (!isExiting.value) {
    isHolding.value = false;
    ringOffset.value = 0; // snap back quickly
  }
}

// Fires only when the overlay's exit animation truly finishes. No ambiguity.
function handleExitComplete() {
  emit("done");
}
</script>

<template>
  <AnimatePresence @exitComplete="handleExitComplete">
    <motion.div
      v-if="!isExiting"
      key="welcome-overlay"
      class="fixed inset-0 z-50 flex items-center justify-center bg-bg-primary"
      :initial="{ opacity: 0, scale: 1.04, filter: 'blur(16px)' }"
      :animate="{ opacity: 1, scale: 1, filter: 'blur(0px)' }"
      :exit="{ opacity: 0, scale: 1.08, filter: 'blur(24px)' }"
      :transition="{ duration: 0.9, ease: [0.22, 1, 0.36, 1] }"
    >
      <motion.button
        type="button"
        class="initicon relative flex touch-none select-none items-center justify-center rounded-full transition-shadow duration-500 focus:outline-none focus:ring-4 focus:ring-pink-deep/20"
        :disabled="isExiting"
        :animate="
          isExiting
            ? {
                scale: [1, 0.85, 1.4, 0],
                y: [0, 8, -40, -120],
                opacity: [1, 1, 0.6, 0],
                rotate: [0, -10, 10, 0],
              }
            : isHolding
              ? { scale: 0.92, y: 0 }
              : { y: [0, -12, 0], scale: [1, 1.06, 1] }
        "
        :transition="
          isExiting
            ? { duration: 1, times: [0, 0.15, 0.5, 1], ease: 'easeOut' }
            : isHolding
              ? { duration: 0.25, ease: 'easeOut' }
              : { duration: 3, repeat: Infinity, ease: 'easeInOut' }
        "
        @pointerdown="startHold"
        @pointerup="cancelHold"
        @pointerleave="cancelHold"
        @pointercancel="cancelHold"
      >
        <!-- shrinking circular hold-loader -->
        <svg
          class="pointer-events-none absolute inset-0 h-full w-full -rotate-90"
          viewBox="0 0 200 200"
        >
          <circle
            cx="100"
            cy="100"
            :r="RADIUS"
            fill="none"
            stroke="currentColor"
            stroke-width="15"
            stroke-linecap="round"
            class="text-pink-deep/70"
            :stroke-dasharray="CIRCUMFERENCE"
            :style="{
              strokeDashoffset: ringOffset,
              transitionProperty: 'stroke-dashoffset',
              transitionDuration: isHolding ? `${HOLD_DURATION}ms` : '200ms',
              transitionTimingFunction: isHolding ? 'linear' : 'ease-out',
            }"
          />
        </svg>

        <Icon class="h-24 w-24 text-pink-deep" icon="line-md:heart-filled" />
      </motion.button>
    </motion.div>
  </AnimatePresence>
</template>
