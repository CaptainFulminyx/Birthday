<template>
  <div class="cake-container">
    <div class="cake">
      <!-- Candle -->
      <div class="candle" :class="{ extinguished: isExtinguished }">
        <div class="flame" v-if="!isExtinguished">
          <div class="flame-inner"></div>
        </div>
        <div class="wick"></div>
        <div class="candle-body"></div>
        <div
          class="drip"
          v-for="n in 3"
          :key="n"
          :style="{ left: n * 25 + '%' }"
        ></div>
      </div>

      <!-- Cake layers -->
      <div class="layer layer-top">
        <div
          class="frosting-swirl"
          v-for="n in 6"
          :key="'swirl-' + n"
          :style="{ left: n * 16 + '%', transform: `rotate(${n * 30}deg)` }"
        ></div>
      </div>
      <div class="layer layer-middle">
        <div class="filling-line"></div>
      </div>
      <div class="layer layer-bottom"></div>

      <!-- Cake base -->
      <div class="cake-base">
        <div class="base-decoration"></div>
      </div>
    </div>

    <!-- Status indicator -->
    <div class="status" :class="{ extinguished: isExtinguished }">
      <span v-if="!isExtinguished">✨ ${Math.ceil(timeLeft)}s</span>
      <span v-else>🎉 Extinguished!</span>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from "vue";

const isExtinguished = ref(false);
const timeLeft = ref(5);
let timer = null;
let countdown = null;

const startCountdown = () => {
  timeLeft.value = 5;
  isExtinguished.value = false;

  countdown = setInterval(() => {
    timeLeft.value -= 0.1;
    if (timeLeft.value <= 0) {
      clearInterval(countdown);
      isExtinguished.value = true;
    }
  }, 100);

  // Safety fallback
  timer = setTimeout(() => {
    isExtinguished.value = true;
    if (countdown) clearInterval(countdown);
  }, 5000);
};

onMounted(() => {
  startCountdown();
});

onBeforeUnmount(() => {
  if (timer) clearTimeout(timer);
  if (countdown) clearInterval(countdown);
});
</script>

<style scoped>
.cake-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  background: #faf5f5;
  padding: 20px;
  font-family: "Georgia", serif;
}

.cake {
  position: relative;
  width: 300px;
  display: flex;
  flex-direction: column;
  align-items: center;
  animation: float 3s ease-in-out infinite;
}

@keyframes float {
  0%,
  100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-10px);
  }
}

/* Candle Styles */
.candle {
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: -8px;
  z-index: 10;
  transition: all 0.5s ease;
}

.candle.extinguished {
  opacity: 0.7;
  transform: scale(0.95);
}

.flame {
  position: relative;
  width: 16px;
  height: 30px;
  background: #ffb3b3;
  border: 2px solid #ff8a8a;
  border-radius: 50% 50% 50% 50% / 60% 60% 40% 40%;
  animation: flicker 0.3s ease-in-out infinite alternate;
  margin-bottom: -4px;
}

.flame-inner {
  position: absolute;
  top: 4px;
  left: 50%;
  transform: translateX(-50%);
  width: 8px;
  height: 20px;
  background: #ffeb99;
  border-radius: 50% 50% 50% 50% / 60% 60% 40% 40%;
  animation: flicker-inner 0.2s ease-in-out infinite alternate;
}

@keyframes flicker {
  0% {
    transform: rotate(-3deg) scaleY(1);
  }
  100% {
    transform: rotate(3deg) scaleY(1.05);
  }
}

@keyframes flicker-inner {
  0% {
    transform: translateX(-50%) scaleY(1) rotate(-2deg);
  }
  100% {
    transform: translateX(-50%) scaleY(0.95) rotate(2deg);
  }
}

.wick {
  width: 2px;
  height: 8px;
  background: #4a4a4a;
  margin-bottom: -2px;
  border-radius: 1px;
}

.candle-body {
  width: 14px;
  height: 40px;
  background: #ffcccc;
  border: 2px solid #ffb3b3;
  border-radius: 3px 3px 2px 2px;
  position: relative;
}

.drip {
  position: absolute;
  bottom: -2px;
  width: 4px;
  height: 10px;
  background: #ffb3b3;
  border: 1px solid #ff9999;
  border-radius: 0 0 3px 3px;
  border-top: none;
}

.drip:nth-child(4) {
  left: 15%;
  height: 7px;
}
.drip:nth-child(5) {
  left: 65%;
  height: 12px;
}
.drip:nth-child(6) {
  left: 85%;
  height: 6px;
}

/* Cake Layers */
.layer {
  width: 100%;
  border: 2px solid #f0d0d0;
  position: relative;
}

.layer-top {
  height: 50px;
  background: #ffd9d9;
  border-radius: 50% 50% 5px 5px / 20% 20% 5px 5px;
  border-bottom: none;
  position: relative;
  overflow: visible;
}

.frosting-swirl {
  position: absolute;
  top: -6px;
  width: 20px;
  height: 12px;
  background: #ffb3b3;
  border: 2px solid #f0a0a0;
  border-radius: 50%;
  transform-origin: center;
}

.frosting-swirl::after {
  content: "";
  position: absolute;
  top: 2px;
  left: 4px;
  width: 8px;
  height: 8px;
  background: #ffcccc;
  border-radius: 50%;
}

.layer-middle {
  height: 40px;
  background: #ffe6e6;
  border-top: none;
  border-bottom: none;
  position: relative;
}

.filling-line {
  position: absolute;
  top: 50%;
  left: 10%;
  right: 10%;
  height: 3px;
  background: #ffb3b3;
  border: 1px solid #f09a9a;
  border-radius: 2px;
  transform: translateY(-50%);
}

.filling-line::before,
.filling-line::after {
  content: "";
  position: absolute;
  top: -6px;
  width: 6px;
  height: 6px;
  background: #ffcccc;
  border: 1px solid #f09a9a;
  border-radius: 50%;
}

.filling-line::before {
  left: -8px;
}
.filling-line::after {
  right: -8px;
}

.layer-bottom {
  height: 55px;
  background: #ffd9d9;
  border-radius: 5px 5px 50% 50% / 5px 5px 20% 20%;
  border-top: none;
}

/* Cake Base */
.cake-base {
  width: 320px;
  height: 12px;
  background: #ffcccc;
  border: 2px solid #f0b0b0;
  border-radius: 50%;
  margin-top: -2px;
  position: relative;
}

.base-decoration {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 60%;
  height: 2px;
  background: #ffb3b3;
  border-radius: 1px;
}

.base-decoration::before,
.base-decoration::after {
  content: "";
  position: absolute;
  top: -4px;
  width: 4px;
  height: 8px;
  background: #ffb3b3;
  border: 1px solid #f09a9a;
  border-radius: 2px;
}

.base-decoration::before {
  left: -8px;
}
.base-decoration::after {
  right: -8px;
}

/* Status */
.status {
  margin-top: 40px;
  padding: 12px 30px;
  background: #ffffff;
  border: 2px solid #ffcccc;
  border-radius: 30px;
  font-size: 18px;
  color: #cc7a7a;
  letter-spacing: 1px;
  transition: all 0.5s ease;
}

.status.extinguished {
  border-color: #e0d0d0;
  background: #f8f0f0;
  color: #b09a9a;
}

.status span {
  display: inline-block;
  min-width: 120px;
  text-align: center;
}

/* Responsive */
@media (max-width: 400px) {
  .cake {
    width: 240px;
  }
  .cake-base {
    width: 260px;
  }
  .layer-top {
    height: 40px;
  }
  .layer-middle {
    height: 32px;
  }
  .layer-bottom {
    height: 45px;
  }
  .candle-body {
    height: 32px;
    width: 12px;
  }
  .flame {
    height: 24px;
    width: 14px;
  }
  .status {
    font-size: 14px;
    padding: 10px 20px;
  }
}
</style>
