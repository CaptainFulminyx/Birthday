<template>
  <div class="cake-container">
    <div class="cake-card">
      <div class="cake-wrapper">
        <svg
          viewBox="0 0 235.9 297.44"
          class="cake-svg"
          aria-label="Interactive Birthday Cake"
        >
          <defs>
            <!-- Flame Soft Radial Glow -->
            <radialGradient id="flameGlow" cx="50%" cy="50%" r="50%">
              <stop offset="0%" stop-color="#FF9D00" stop-opacity="0.4" />
              <stop offset="100%" stop-color="#FF9D00" stop-opacity="0" />
            </radialGradient>
          </defs>

          <!-- Ambient Glow Layer -->
          <circle
            v-if="candleLit && !extinguishing"
            cx="139.1"
            cy="60"
            r="50"
            fill="url(#flameGlow)"
            class="ambient-glow"
          />

          <!-- 
            YOUR ORIGINAL SVG PATHS
            All paths preserved exactly as provided
          -->
          <path
            d="M63.87 231.68 L63.87 259.25 C69.91 278.95 206.65 279.52 213.82 262.32 L213.82 231.97"
            fill-rule="evenodd"
            style="
              fill: #d4c871;
              stroke: #000000;
              stroke-linecap: round;
              stroke-linejoin: round;
              stroke-width: 4.04;
            "
          />

          <!-- Cake Top Layer (Frosting) -->
          <path
            d="M64.16 173.3 L63.87 231.68 C119.81 236.06 161.49 240.93 213.82 231.97 L214.05 177.13"
            style="
              fill: #faedcd;
              stroke: #000000;
              stroke-linecap: round;
              stroke-linejoin: round;
              stroke-width: 4.04;
            "
          />

          <!-- Decorative Sprinkles/Stars -->
          <path
            d="M50.17 78.42 L79.71 86.18 L76.38 58.08 L90.95 31.57 L61.34 28.03 L44.52 2.02 L31.17 29.39 L2.02 39.13 L21.05 59.44 L21.05 89.77 L50.17 78.42"
            style="
              fill: #88ff5f;
              stroke: #000000;
              stroke-linecap: round;
              stroke-linejoin: round;
              stroke-width: 4.04;
            "
          />

          <!-- Decorative Eyes -->
          <ellipse
            transform="matrix(0.0436793455772179, 0, 0, 0.0436793455772179, 120.660779781565, 205.15169228311)"
            cx="0"
            cy="0"
            rx="100"
            ry="100"
            style="fill: #000000"
          />
          <ellipse
            transform="matrix(0.0436793455772179, 0, 0, 0.0436793455772179, 160.360800988913, 205.15169228311)"
            cx="0"
            cy="0"
            rx="100"
            ry="100"
            style="fill: #000000"
          />

          <!-- Smile -->
          <path
            d="M129.31 216.27 Q140.82 228.55 152.28 216.36"
            style="
              fill: none;
              stroke: #000000;
              stroke-linecap: round;
              stroke-linejoin: round;
              stroke-width: 4.04;
            "
          />

          <!-- 🔥 FLAME (Our animation target) -->
          <g v-if="candleLit || extinguishing">
            <path
              id="flame"
              d="M139.1 82.86 
     C132 78, 125 70, 139.1 55
     C153 70, 146 78, 139.1 82.86 Z"
              style="
                fill: #ff0;
                stroke: #000;
                stroke-linecap: round;
                stroke-linejoin: round;
                stroke-width: 4.04;
              "
              class="flame-path"
              :class="{ 'flame-extinguishing': extinguishing }"
            />
          </g>

          <!-- 💨 SMOKE PARTICLES (Appears when extinguished) -->
          <g v-if="showSmoke" class="smoke-group">
            <!-- Smoke Path 1 -->
            <path
              class="smoke-particle p1"
              d="M139.1 82.86 Q135 70 138 55 T136 40"
              fill="none"
              stroke="#D3D3D3"
              stroke-width="3"
              stroke-linecap="round"
            />
            <!-- Smoke Path 2 -->
            <path
              class="smoke-particle p2"
              d="M139.1 82.86 Q143 72 140 57 T142 42"
              fill="none"
              stroke="#E8E8E8"
              stroke-width="2.5"
              stroke-linecap="round"
            />
            <!-- Smoke Path 3 -->
            <path
              class="smoke-particle p3"
              d="M139.1 82.86 Q137 68 141 53 T139 38"
              fill="none"
              stroke="#BEBEBE"
              stroke-width="2"
              stroke-linecap="round"
            />
          </g>

          <!-- Candle Body (Your existing candle path) -->
          <path
            id="candle-body"
            d="M128.11 142.66 L128.82 103.02 C132.88 98.31 145.46 97.91 149.83 103.02 L150.01 144.19 M139.1 82.86 L139.1 99"
            style="
              fill: #ffafcc;
              stroke: #000000;
              stroke-linecap: round;
              stroke-linejoin: round;
              stroke-width: 4.04;
            "
          />

          <!-- Decorative Elements -->
          <path
            d="M213.76 185.71 C221.14 174.7 220.61 163.95 214.05 154.95 C178.08 139.43 99.11 139.15 64.16 151.04 C51.94 163.66 55.77 180.84 61.79 183.54 C67.07 186.94 76.06 185.6 83.15 179.05 C96.91 190.99 109.55 191.62 120.66 177.99 C136.29 192.02 149.63 191.39 160.36 176.76 C172.37 190.16 186.3 189.82 195.18 177.13 C197.47 182.29 203.56 186.96 213.76 185.71"
            style="
              fill: #9b9a9c;
              stroke: #000000;
              stroke-linecap: round;
              stroke-linejoin: round;
              stroke-width: 4.04;
            "
          />

          <path
            d="M50.42 79.89 C49.48 106.64 41.25 125.33 32.41 154.55 C23.8 201.31 50.17 201.31 32.41 240.05"
            fill-rule="evenodd"
            style="
              fill: none;
              stroke: #000000;
              stroke-linecap: round;
              stroke-linejoin: round;
              stroke-width: 4.04;
            "
          />

          <path
            d="M38.19 209.69 C47.01 219.18 55.13 226.65 63.9 225.65 M215.46 225.7 C229.69 229.98 235.25 239.85 233.59 254.37"
            fill-rule="evenodd"
            style="
              fill: none;
              stroke: #000000;
              stroke-linecap: round;
              stroke-linejoin: round;
              stroke-width: 4.04;
            "
          />

          <path
            d="M113.84 273.83 L113.84 294.91 M159.63 275.67 L159.63 295.42"
            fill-rule="evenodd"
            style="
              fill: none;
              stroke: #000000;
              stroke-linecap: round;
              stroke-linejoin: round;
              stroke-width: 4.04;
            "
          />
        </svg>
      </div>

      <!-- Control Section -->
      <div class="controls-area">
        <!-- SVG Radial Countdown Ring -->
        <div class="countdown-wrapper" v-if="showCountdown">
          <svg class="countdown-svg" viewBox="0 0 40 40">
            <circle class="countdown-bg" cx="20" cy="20" r="16" />
            <circle
              class="countdown-progress"
              cx="20"
              cy="20"
              r="16"
              :style="{ strokeDashoffset: progressOffset }"
            />
          </svg>
          <span class="count">{{ Math.ceil(timeLeft) }}</span>
        </div>

        <!-- Action Buttons -->
        <button
          class="action-btn blow-btn"
          @click="handleBlowOut"
          v-if="candleLit && !isAnimating"
        >
          <span>🌬️</span> Blow Out!
        </button>

        <button
          class="action-btn reset-btn"
          @click="resetCandle"
          v-if="!candleLit"
        >
          <span>✨</span> Relight Candle
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted, nextTick } from "vue";

const TOTAL_TIME = 5;
const candleLit = ref(true);
const isAnimating = ref(false);
const extinguishing = ref(false);
const showSmoke = ref(false);
const showCountdown = ref(true);
const timeLeft = ref(TOTAL_TIME);

let countdownInterval = null;
let smokeAnimationTimer = null;

const CIRCUMFERENCE = 100.53;

const progressOffset = computed(() => {
  const progress = timeLeft.value / TOTAL_TIME;
  return CIRCUMFERENCE * (1 - progress);
});

const blowOutCandle = () => {
  if (!candleLit.value || extinguishing.value) return;

  isAnimating.value = true;
  extinguishing.value = true;
  clearInterval(countdownInterval);

  // Wait for the flame to render with the extinguishing class
  nextTick(() => {
    const flame = document.querySelector(".flame-path");
    if (flame) {
      // Set initial transform origin
      flame.style.transformOrigin = "10px 25px";
      flame.style.transformBox = "fill-box";

      // Apply the extinguishing animation
      flame.classList.add("flame-extinguishing");
    }
  });

  // After flame animation completes, show smoke
  setTimeout(() => {
    candleLit.value = false;
    extinguishing.value = false;
    showSmoke.value = true;
    showCountdown.value = false;
    isAnimating.value = false;

    // Trigger smoke animation
    nextTick(() => {
      animateSmoke();
    });
  }, 600);
};

const animateSmoke = () => {
  const particles = document.querySelectorAll(".smoke-particle");

  particles.forEach((particle, index) => {
    // Reset position
    particle.style.opacity = "0";
    particle.style.transform = "none";
    particle.style.transition = "none";

    // Force reflow
    void particle.offsetHeight;

    // Animate with staggered delays
    const delay = index * 200;

    setTimeout(() => {
      // Set initial state
      particle.style.transition = `opacity ${0.5 + index * 0.2}s ease-in, transform ${2 + index * 0.5}s ease-out`;
      particle.style.opacity = "0.7";

      // Apply vertical rise animation using CSS transform
      particle.style.transform = `translateY(-${30 + index * 15}px)`;
      particle.style.transformOrigin = "139.1px 82.86px";
      particle.style.transformBox = "fill-box";

      // Fade out after rising
      setTimeout(
        () => {
          particle.style.transition = `opacity ${0.5 + index * 0.3}s ease-out`;
          particle.style.opacity = "0";
        },
        1500 + index * 300,
      );
    }, delay);
  });
};

const handleBlowOut = () => {
  if (candleLit.value && !extinguishing.value) {
    blowOutCandle();
  }
};

const resetCandle = () => {
  // Reset smoke particles
  const particles = document.querySelectorAll(".smoke-particle");
  particles.forEach((p) => {
    p.style.opacity = "0";
    p.style.transform = "none";
    p.style.transition = "none";
  });

  showSmoke.value = false;
  candleLit.value = true;
  startCountdown();
};

const startCountdown = () => {
  clearInterval(countdownInterval);
  timeLeft.value = TOTAL_TIME;
  showCountdown.value = true;
  isAnimating.value = false;
  extinguishing.value = false;

  countdownInterval = setInterval(() => {
    timeLeft.value -= 0.05;

    if (timeLeft.value <= 0) {
      timeLeft.value = 0;
      clearInterval(countdownInterval);
      handleBlowOut();
    }
  }, 50);
};

onMounted(() => {
  startCountdown();
});

// Clean up
onUnmounted(() => {
  clearInterval(countdownInterval);
  clearTimeout(smokeAnimationTimer);
});
</script>

<style scoped>
.cake-container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background: #f0f0f0;
}

.cake-card {
  background: white;
  border-radius: 20px;
  padding: 20px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
}

.cake-wrapper {
  max-width: 300px;
  margin: 0 auto;
}

.cake-svg {
  width: 100%;
  height: auto;
}

/* Flame animation */
.flame-path {
  transition: all 0.3s ease-out;
  transform-origin: 139.1px 82.86px;
  transform-box: fill-box;
}

.flame-extinguishing {
  animation: flameOut 0.6s ease-in forwards;
}

@keyframes flameOut {
  0% {
    transform: scale(1, 1);
    opacity: 1;
  }
  30% {
    transform: scale(1.1, 0.85) rotate(-8deg);
    opacity: 0.9;
  }
  60% {
    transform: scale(0.5, 0.3) rotate(4deg) translateY(2px);
    opacity: 0.5;
  }
  100% {
    transform: scale(0.1, 0.05) rotate(4deg) translateY(6px);
    opacity: 0;
  }
}

/* Smoke animations */
.smoke-particle {
  transition: all 0.3s ease;
  transform-origin: 139.1px 82.86px;
  transform-box: fill-box;
  opacity: 0;
}

.smoke-particle.p1 {
  animation: smokeRise 2.5s ease-out infinite;
  animation-delay: 0s;
}

.smoke-particle.p2 {
  animation: smokeRise 3s ease-out infinite;
  animation-delay: 0.3s;
}

.smoke-particle.p3 {
  animation: smokeRise 3.5s ease-out infinite;
  animation-delay: 0.6s;
}

@keyframes smokeRise {
  0% {
    transform: translateY(0) scale(1);
    opacity: 0;
  }
  20% {
    opacity: 0.7;
  }
  60% {
    opacity: 0.4;
  }
  100% {
    transform: translateY(-40px) scale(1.5);
    opacity: 0;
  }
}

/* Controls */
.controls-area {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
  margin-top: 20px;
}

.countdown-wrapper {
  position: relative;
  width: 50px;
  height: 50px;
}

.countdown-svg {
  width: 100%;
  height: 100%;
  transform: rotate(-90deg);
}

.countdown-bg {
  fill: none;
  stroke: #e0e0e0;
  stroke-width: 3;
}

.countdown-progress {
  fill: none;
  stroke: #4caf50;
  stroke-width: 3;
  stroke-linecap: round;
  transition: stroke-dashoffset 0.1s linear;
}

.count {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  font-size: 16px;
  font-weight: bold;
  color: #333;
}

.action-btn {
  padding: 10px 20px;
  border: none;
  border-radius: 25px;
  font-size: 16px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-weight: bold;
}

.blow-btn {
  background: #ff6b6b;
  color: white;
}

.blow-btn:hover {
  background: #ff5252;
  transform: scale(1.05);
}

.reset-btn {
  background: #4caf50;
  color: white;
}

.reset-btn:hover {
  background: #45a049;
  transform: scale(1.05);
}

.ambient-glow {
  animation: pulse 2s ease-in-out infinite;
}

@keyframes pulse {
  0%,
  100% {
    opacity: 1;
  }
  50% {
    opacity: 0.6;
  }
}
</style>
