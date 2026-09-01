<script setup>
import { ref, onMounted, onUnmounted } from "vue";
import { animate, createTimeline, stagger, splitText } from "animejs";
import Hearts from "$/Hearts.vue";

const isRevealed = ref(false);
const revealWrapper = ref(null);

// Double-tap detection state
let lastTapTime = 0;
let isAnimatingReveal = false;
const DOUBLE_TAP_DELAY = 300;

const handleDoubleTap = () => {
  if (isAnimatingReveal) return;

  isAnimatingReveal = true;
  isRevealed.value = !isRevealed.value;

  animate(".reveal-layer", {
    clipPath: isRevealed.value
      ? ["circle(0% at 50% 50%)", "circle(150% at 50% 50%)"]
      : ["circle(150% at 50% 50%)", "circle(0% at 50% 50%)"],
    ease: "outQuad",
    duration: 1000,
    complete: () => {
      isAnimatingReveal = false;
    },
  });
};

const onTapStart = (e) => {
  // Prevent ghost clicks on touch devices when using pointer/touch events
  if (e.type === "click" && "ontouchstart" in window) return;
  if (isAnimatingReveal) return;

  const now = Date.now();
  if (now - lastTapTime < DOUBLE_TAP_DELAY) {
    lastTapTime = 0;
    handleDoubleTap();
  } else {
    lastTapTime = now;
  }
};

onMounted(() => {
  const el = revealWrapper.value;

  // Initialize split text animation AFTER DOM is mounted
  const { words, chars } = splitText(".heading", {
    words: { wrap: "clip" },
    chars: true,
  });

  createTimeline({
    loop: false,
    defaults: { ease: "inOut(2)", duration: 650 },
  })
    .add(
      chars,
      {
        y: {
          from: "-250px",
          to: "0px",
        },
      },
      stagger(125),
    )
    .add(
      words,
      {
        y: {
          from: "-250px",
          to: "0px",
        },
      },
      stagger(125),
    )
    .init();

  // Attach tap listeners safely
  el?.addEventListener("touchstart", onTapStart, { passive: true });
  el?.addEventListener("click", onTapStart);
});

onUnmounted(() => {
  const el = revealWrapper.value;
  el?.removeEventListener("touchstart", onTapStart);
  el?.removeEventListener("click", onTapStart);
});
</script>

<template>
  <div class="page">
    <h1 class="heading">
      Hellooo,<br />
      Birthday Girl!!!
    </h1>

    <div class="body">
      <svg
        class="doodle"
        xmlns="http://www.w3.org/2000/svg"
        viewBox="0 0 505.57 1124.97"
        preserveAspectRatio="xMidYMid meet"
      >
        <path
          transform="matrix(-0.999999980062419, -0.0001996876628125, 0.000199687662812962, -0.999999980062419, 133.043446200906, 126.588123680163)"
          d="M-136.67 110.99 C-455.61 -82.02 -335.62 -436.54 -61.31 -436.59 C150.04 -436.63 150.16 -152.68 -47.32 -131.44 C-447.92 -92.31 -525.4 -891.69 117.28 -891.69 L9.62 -982.74 L117.28 -891.69 L9.62 -792.41"
          fill-rule="evenodd"
          style="
            fill: none;
            stroke: currentColor;
            stroke-linecap: round;
            stroke-linejoin: round;
            stroke-width: 40;
          "
        />
      </svg>

      <p class="message">
        Hey you little sweety pie, guess who's the queen today? It's my gorgeous
        baby girl &lt;3
      </p>
    </div>

    <Hearts />

    <!-- Card Container -->
    <div class="card-container">
      <div ref="revealWrapper" class="reveal-wrapper">
        <!-- Base Layer -->
        <div class="base-layer">
          <h3 class="swipe-hint">Double Tap</h3>
          <img
            src="../assets/ghostwithcake.jpg"
            class="ghost-img"
            alt="Ghost with cake"
          />
        </div>

        <!-- Revealed Layer -->
        <div class="reveal-layer">
          <h1 class="heading reveal-heading">
            Happy <br />
            Birthday
          </h1>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.page {
  max-width: 100vw;
  max-height: 100vh;
  margin: 0 auto;
  padding: 2.5rem 2rem 3rem 2rem;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.heading {
  font-size: 4rem;
  font-weight: 800;
  line-height: 1.3;
  color: #e8477a;
  padding-left: 30px;
  margin: 0 0 0.5rem;
  text-align: left;
  transform: rotate(-5deg) scale(1.3);
}

.body {
  position: relative;
  padding-top: 0.5rem;
}

.doodle {
  width: 35px;
  height: auto;
  color: #e8477a;
  opacity: 1;
  float: right;
  transform: rotate(25deg) translate(-45px, 0px) scale(0.97);
  margin-left: 0rem;
}

.message {
  font-size: 1.15rem;
  line-height: 1.1;
  color: #5c3a44;
  margin: 0;
  padding-right: 50px;
  padding-top: 20px;
}

.card-container {
  touch-action: none;
  user-select: none;
  cursor: grab;
  margin-top: 20px;
  will-change: transform;
}

.card-container:active {
  cursor: grabbing;
}

.reveal-wrapper {
  position: relative;
  border-radius: 20px;
  border: 5px solid var(--color-pink-deep, #e8477a);
  overflow: hidden;
  width: 350px;
  height: 350px;
}

.base-layer {
  width: 100%;
  height: 100%;
  position: relative;
}

.swipe-hint {
  position: absolute;
  right: 20px;
  top: 10px;
  margin: 0;
  z-index: 2;
  color: #fff;
  text-shadow: 0 0 5px rgba(0, 0, 0, 0.5);
}

.ghost-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.reveal-layer {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: var(--color-pink-deep, #e8477a);
  display: flex;
  justify-content: center;
  align-items: center;
  clip-path: circle(0% at 50% 50%);
  z-index: 3;
}

.reveal-heading {
  font-size: 4rem;
  text-align: center;
  margin: 0;
  padding: 0;
  transform: rotate(-3deg);
  color: #fff;
}

@media (max-width: 400px) {
  .heading {
    font-size: 2.2rem;
  }
  .message {
    font-size: 1rem;
  }
  .reveal-wrapper {
    width: 300px;
    height: 300px;
  }
  .reveal-heading {
    font-size: 4rem;
  }
}
</style>
