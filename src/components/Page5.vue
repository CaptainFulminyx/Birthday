<template>
  <div class="cake-container">
    <div class="cake-card">
      <div class="cake-wrapper">
        <svg
          viewBox="0 0 300 360"
          class="cake-svg"
          aria-label="Interactive Birthday Cake"
        >
          <defs>
            <!-- Flame Soft Radial Glow -->
            <radialGradient id="flameGlow" cx="50%" cy="50%" r="50%">
              <stop offset="0%" stop-color="#FF9D00" stop-opacity="0.4" />
              <stop offset="100%" stop-color="#FF9D00" stop-opacity="0" />
            </radialGradient>

            <!-- Cake Base Gradient -->
            <linearGradient id="cakeBaseGrad" x1="0%" y1="0%" x2="100%" y2="0%">
              <stop offset="0%" stop-color="#F2D7B6" />
              <stop offset="50%" stop-color="#E8C591" />
              <stop offset="100%" stop-color="#DDA96C" />
            </linearGradient>

            <!-- Frosting Gradient -->
            <linearGradient id="frostingGrad" x1="0%" y1="0%" x2="0%" y2="100%">
              <stop offset="0%" stop-color="#FFF0F5" />
              <stop offset="100%" stop-color="#F5C2D0" />
            </linearGradient>

            <!-- Candle Stripe Pattern -->
            <pattern
              id="candleStripe"
              width="8"
              height="8"
              patternTransform="rotate(45 0 0)"
              patternUnits="userSpaceOnUse"
            >
              <line
                x1="0"
                y1="0"
                x2="0"
                y2="8"
                stroke="#E65100"
                stroke-width="4"
              />
              <line
                x1="4"
                y1="0"
                x2="4"
                y2="8"
                stroke="#FFF"
                stroke-width="4"
              />
            </pattern>
          </defs>

          <!-- Ambient Glow Layer -->
          <circle
            v-if="candleLit && !extinguishing"
            cx="150"
            cy="70"
            r="80"
            fill="url(#flameGlow)"
            class="ambient-glow"
          />

          <!-- Cake Plate -->
          <ellipse cx="150" cy="305" rx="115" ry="14" fill="#E2E8F0" />
          <ellipse
            cx="150"
            cy="302"
            rx="110"
            ry="12"
            fill="#FFFFFF"
            stroke="#CBD5E1"
            stroke-width="2"
          />

          <!-- Bottom Layer -->
          <!--  <path
            d="M 50 230 Q 150 245 250 230 L 250 290 Q 150 305 50 290 Z"
            fill="url(#cakeBaseGrad)"
            stroke="#2C2C2C"
            stroke-width="3"
            stroke-linejoin="round"
          />-->

          <!-- Mid Cream Layer Fill -->
          <path
            d="M 50 260 Q 150 273 250 260"
            stroke="#FFF"
            stroke-width="6"
            fill="none"
            opacity="0.9"
          />

          <!-- Top Layer Cake Base -->
          <path
            d="M 70 170 Q 150 182 230 170 L 230 235 Q 150 248 70 235 Z"
            fill="url(#cakeBaseGrad)"
            stroke="#2C2C2C"
            stroke-width="3"
            stroke-linejoin="round"
          />

          <!-- Frosting Top Drips -->
          <path
            d="M 68 170 
               Q 68 150 150 150 
               Q 232 150 232 170 
               Q 220 195 210 175 
               Q 195 200 180 175 
               Q 165 205 150 175 
               Q 135 200 120 175 
               Q 105 195 90 175 
               Q 78 190 68 170 Z"
            fill="url(#frostingGrad)"
            stroke="#2C2C2C"
            stroke-width="3"
            stroke-linejoin="round"
          />

          <!-- Decorative Sprinkles -->
          <g class="sprinkles">
            <rect
              x="95"
              y="162"
              width="6"
              height="3"
              rx="1.5"
              fill="#FF6B6B"
              transform="rotate(15 95 162)"
            />
            <rect
              x="125"
              y="165"
              width="6"
              height="3"
              rx="1.5"
              fill="#4ECDC4"
              transform="rotate(-30 125 165)"
            />
            <rect
              x="175"
              y="163"
              width="6"
              height="3"
              rx="1.5"
              fill="#FFE66D"
              transform="rotate(45 175 163)"
            />
            <rect
              x="205"
              y="165"
              width="6"
              height="3"
              rx="1.5"
              fill="#FF6B6B"
              transform="rotate(-15 205 165)"
            />
          </g>

          <!-- Candle Body -->
          <rect
            x="143"
            y="90"
            width="14"
            height="62"
            rx="3"
            fill="url(#candleStripe)"
            stroke="#2C2C2C"
            stroke-width="2.5"
          />

          <!-- Candle Wick -->
          <path
            d="M 150 90 Q 148 83 150 77"
            fill="none"
            stroke="#2C2C2C"
            stroke-width="2.5"
            stroke-linecap="round"
          />

          <!-- Flame Assembly -->
          <g v-if="candleLit" class="flame-group" :class="{ extinguishing }">
            <!-- Flame Outer -->
            <path
              d="M 150 77 C 137 60 142 35 150 20 C 158 35 163 60 150 77 Z"
              fill="#FF9800"
              stroke="#E65100"
              stroke-width="1.5"
            />
            <!-- Flame Middle -->
            <path
              d="M 150 75 C 142 62 145 42 150 30 C 155 42 158 62 150 75 Z"
              fill="#FFEB3B"
            />
            <!-- Flame Core -->
            <path
              d="M 150 73 C 146 65 147 50 150 42 C 153 50 154 65 150 73 Z"
              fill="#FFFFFF"
            />
          </g>

          <!-- Smoke Particles (Appears when extinguished) -->
          <g v-if="!candleLit" class="smoke-group">
            <path
              class="smoke-particle p1"
              d="M 150 75 Q 145 55 152 35 T 148 15"
              fill="none"
              stroke="#A0AEC0"
              stroke-width="2"
              stroke-linecap="round"
            />
            <path
              class="smoke-particle p2"
              d="M 150 75 Q 155 60 147 40 T 153 20"
              fill="none"
              stroke="#CBD5E1"
              stroke-width="1.5"
              stroke-linecap="round"
            />
          </g>

          <!-- Cute Face Details -->
          <g class="cake-face">
            <circle cx="130" cy="205" r="3.5" fill="#2C2C2C" />
            <circle cx="170" cy="205" r="3.5" fill="#2C2C2C" />
            <!-- Cheeks -->
            <circle cx="122" cy="210" r="4.5" fill="#FF8A8A" opacity="0.5" />
            <circle cx="178" cy="210" r="4.5" fill="#FF8A8A" opacity="0.5" />
            <!-- Smile -->
            <path
              d="M 143 212 Q 150 217 157 212"
              fill="none"
              stroke="#2C2C2C"
              stroke-width="2"
              stroke-linecap="round"
            />
          </g>
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
          @click="blowOutCandle"
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
import { ref, computed, onMounted, onUnmounted } from "vue";

const TOTAL_TIME = 5;
const candleLit = ref(true);
const isAnimating = ref(false);
const extinguishing = ref(false);
const showCountdown = ref(true);
const timeLeft = ref(TOTAL_TIME);

let countdownInterval = null;

// Circumference of SVG circle with r=16 (2 * PI * 16 ≈ 100.53)
const CIRCUMFERENCE = 100.53;

const progressOffset = computed(() => {
  const progress = timeLeft.value / TOTAL_TIME;
  return CIRCUMFERENCE * (1 - progress);
});

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
      blowOutCandle();
    }
  }, 50);
};

const blowOutCandle = () => {
  if (!candleLit.value || extinguishing.value) return;

  isAnimating.value = true;
  extinguishing.value = true;
  clearInterval(countdownInterval);

  setTimeout(() => {
    candleLit.value = false;
    extinguishing.value = false;
    showCountdown.value = false;
    isAnimating.value = false;
  }, 500);
};

const resetCandle = () => {
  candleLit.value = true;
  startCountdown();
};

onMounted(() => {
  startCountdown();
});

onUnmounted(() => {
  clearInterval(countdownInterval);
});
</script>

<style scoped>
/* Page Layout */
.cake-container {
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  background: linear-gradient(135deg, #fdfbfb 0%, #ebedee 100%);
  padding: 1.5rem;
  font-family:
    system-ui,
    -apple-system,
    BlinkMacSystemFont,
    "Segoe UI",
    Roboto,
    sans-serif;
}

/* Glassmorphism Card Wrapper */
.cake-card {
  background: rgba(255, 255, 255, 0.85);
  backdrop-filter: blur(12px);
  border: 1px solid rgba(255, 255, 255, 0.6);
  border-radius: 28px;
  padding: 2.5rem 2rem 2rem;
  box-shadow: 0 20px 40px -15px rgba(0, 0, 0, 0.07);
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  max-width: 380px;
}

.cake-wrapper {
  width: 100%;
  aspect-ratio: 1;
  display: flex;
  align-items: center;
  justify-content: center;
}

.cake-svg {
  width: 100%;
  height: 100%;
  overflow: visible;
}

/* Animations */
.ambient-glow {
  animation: pulse-glow 2s ease-in-out infinite alternate;
}

@keyframes pulse-glow {
  0% {
    opacity: 0.4;
    transform: scale(0.98);
    transform-origin: 150px 70px;
  }
  100% {
    opacity: 0.8;
    transform: scale(1.05);
    transform-origin: 150px 70px;
  }
}

.flame-group {
  animation: flame-flicker 0.1s infinite alternate
    cubic-bezier(0.45, 0.05, 0.55, 0.95);
  transform-origin: 150px 77px;
  transition:
    opacity 0.4s ease,
    transform 0.4s ease;
}

.flame-group.extinguishing {
  opacity: 0;
  transform: scale(0.1) translateY(-20px);
}

@keyframes flame-flicker {
  0% {
    transform: rotate(-1.5deg) scaleX(0.96) translateY(0);
  }
  100% {
    transform: rotate(1.5deg) scaleX(1.04) translateY(-1px);
  }
}

/* Smoke Effect */
.smoke-particle {
  stroke-dasharray: 40;
  stroke-dashoffset: 40;
  animation: rise-smoke 2s ease-out forwards;
}

.smoke-particle.p2 {
  animation-delay: 0.2s;
}

@keyframes rise-smoke {
  0% {
    stroke-dashoffset: 40;
    opacity: 0.8;
  }
  50% {
    opacity: 0.5;
  }
  100% {
    stroke-dashoffset: 0;
    opacity: 0;
    transform: translateY(-20px);
  }
}

/* Controls & Interactive Elements */
.controls-area {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1.25rem;
  width: 100%;
  margin-top: 0.5rem;
  min-height: 110px;
  justify-content: flex-end;
}

/* Circular Countdown Timer */
.countdown-wrapper {
  position: relative;
  width: 52px;
  height: 52px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.countdown-svg {
  width: 100%;
  height: 100%;
  transform: rotate(-90deg);
}

.countdown-bg {
  fill: none;
  stroke: #edf2f7;
  stroke-width: 3.5;
}

.countdown-progress {
  fill: none;
  stroke: #f5c2d0;
  stroke-width: 3.5;
  stroke-linecap: round;
  stroke-dasharray: 100.53;
  transition: stroke-dashoffset 0.05s linear;
}

.count {
  position: absolute;
  font-size: 1.125rem;
  font-weight: 700;
  color: #4a5568;
  font-variant-numeric: tabular-nums;
}

/* Micro-Interactive Buttons */
.action-btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  width: 100%;
  padding: 0.875rem 1.5rem;
  font-size: 1rem;
  font-weight: 600;
  border-radius: 99px;
  border: none;
  cursor: pointer;
  outline: none;
  transition: all 0.25s cubic-bezier(0.16, 1, 0.3, 1);
}

.blow-btn {
  background: #2c2c2c;
  color: #ffffff;
  box-shadow: 0 4px 12px rgba(44, 44, 44, 0.15);
}

.blow-btn:hover {
  background: #404040;
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(44, 44, 44, 0.25);
}

.blow-btn:active {
  transform: translateY(0);
}

.reset-btn {
  background: #f5c2d0;
  color: #2c2c2c;
  box-shadow: 0 4px 12px rgba(245, 194, 208, 0.3);
}

.reset-btn:hover {
  background: #f0b0c2;
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(245, 194, 208, 0.45);
}

.reset-btn:active {
  transform: translateY(0);
}

/* Responsive adjustments */
@media (max-width: 480px) {
  .cake-card {
    padding: 1.5rem 1.25rem;
    border-radius: 20px;
  }
}
</style>
