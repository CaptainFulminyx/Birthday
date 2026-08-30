<script setup>
import { ref, onMounted, onBeforeUnmount } from "vue";
import { Icon } from "@iconify/vue";
import { animate, stagger, utils } from "animejs";

const emit = defineEmits(["done"]);

const HOLD_DURATION = 1200; // ms to hold before dismissing
const RADIUS = 70;
const CIRCUMFERENCE = 2 * Math.PI * RADIUS;
const TITLE_TEXT = "Hold the Heart <3";

const visible = ref(true);
const isHolding = ref(false);
const isExiting = ref(false);

const containerRef = ref(null);
const buttonRef = ref(null);
const circleRef = ref(null);
const titleRef = ref(null);
const titleChars = TITLE_TEXT.split("");

let idleAnim = null;
let holdAnim = null;

function startIdle() {
  idleAnim = animate(buttonRef.value, {
    translateY: [0, -12, 0],
    scale: [1, 1.06, 1],
    duration: 3000,
    loop: true,
    ease: "inOutQuad",
  });
}

function startHold(e) {
  if (isExiting.value) return;
  e.preventDefault();
  isHolding.value = true;

  idleAnim?.pause();

  animate(buttonRef.value, {
    scale: 0.92,
    translateY: 0,
    duration: 250,
    ease: "outQuad",
  });

  const current =
    parseFloat(utils.get(circleRef.value, "strokeDashoffset")) || CIRCUMFERENCE;

  holdAnim = animate(circleRef.value, {
    strokeDashoffset: 0,
    duration: HOLD_DURATION * (current / CIRCUMFERENCE),
    ease: "linear",
    onComplete: playExit,
  });
}

function cancelHold() {
  holdAnim?.pause();
  if (isExiting.value || !isHolding.value) return;
  isHolding.value = false;

  animate(circleRef.value, {
    strokeDashoffset: CIRCUMFERENCE,
    duration: 500,
    ease: "outQuad",
  });

  animate(buttonRef.value, {
    scale: 1,
    translateY: 0,
    duration: 250,
    ease: "outQuad",
    onComplete: startIdle,
  });
}

function playExit() {
  isHolding.value = false;
  isExiting.value = true;
  idleAnim?.pause();

  // overlay fade/blur out
  animate(containerRef.value, {
    opacity: [1, 0],
    scale: [1, 1.08],
    filter: ["blur(0px)", "blur(24px)"],
    duration: 1000,
    ease: "outQuad",
  });

  // heart flourish, timed to mirror the original [0, 0.15, 0.5, 1] keyframes
  animate(buttonRef.value, {
    scale: [
      { to: 0.85, duration: 150 },
      { to: 1.4, duration: 350 },
      { to: 0, duration: 500 },
    ],
    translateY: [
      { to: 8, duration: 150 },
      { to: -40, duration: 350 },
      { to: -120, duration: 500 },
    ],
    opacity: [
      { to: 1, duration: 150 },
      { to: 0.6, duration: 350 },
      { to: 0, duration: 500 },
    ],
    rotate: [
      { to: -10, duration: 150 },
      { to: 10, duration: 350 },
      { to: 0, duration: 500 },
    ],
    ease: "outQuad",
    onComplete: () => {
      visible.value = false;
      emit("done");
    },
  });
}

onMounted(() => {
  utils.set(circleRef.value, { strokeDashoffset: CIRCUMFERENCE });

  animate(containerRef.value, {
    opacity: [0, 1],
    scale: [1.04, 1],
    filter: ["blur(16px)", "blur(0px)"],
    duration: 900,
    ease: "cubicBezier(.22, 1, .36, 1)",
  });

  animate(titleRef.value.querySelectorAll(".char"), {
    opacity: [0, 1],
    translateY: [16, 0],
    duration: 600,
    delay: stagger(35),
    ease: "outQuad",
  });

  startIdle();
});

onBeforeUnmount(() => {
  idleAnim?.pause();
  holdAnim?.pause();
});
</script>

<template>
  <div
    v-if="visible"
    ref="containerRef"
    class="fixed inset-0 z-50 flex flex-col items-center justify-center gap-10 bg-bg-primary"
    style="opacity: 0"
  >
    <button
      ref="buttonRef"
      type="button"
      class="initicon relative flex touch-none select-none items-center justify-center rounded-full transition-shadow duration-500 focus:outline-none focus:ring-4 focus:ring-pink-deep/20"
      :disabled="isExiting"
      @pointerdown="startHold"
      @pointerup="cancelHold"
      @pointerleave="cancelHold"
      @pointercancel="cancelHold"
    >
      <svg
        class="pointer-events-none absolute inset-0 h-full w-full -rotate-90"
        viewBox="0 0 200 200"
      >
        <circle
          ref="circleRef"
          cx="100"
          cy="100"
          :r="RADIUS"
          fill="none"
          stroke="currentColor"
          stroke-width="15"
          stroke-linecap="round"
          class="text-pink-deep/70"
          :stroke-dasharray="CIRCUMFERENCE"
          :stroke-dashoffset="CIRCUMFERENCE"
        />
      </svg>

      <Icon class="h-24 w-24 text-pink-deep" icon="line-md:heart-filled" />
    </button>

    <h2 ref="titleRef" class="flex flex-wrap justify-center">
      <span
        v-for="(char, i) in titleChars"
        :key="i"
        class="char inline-block"
        style="opacity: 0"
        >{{ char === " " ? "\u00A0" : char }}</span
      >
    </h2>
  </div>
</template>
