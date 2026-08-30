<script setup>
import { ref, onMounted, watch, nextTick } from "vue";
import { animate, stagger, utils } from "animejs";
import Hearts from "../components/Hearts.vue";

const imgs = ref([
  { src: "https://picsum.photos/200/200?random=1", rot: -7 },
  { src: "https://picsum.photos/200/200?random=2", rot: 5 },
  { src: "https://picsum.photos/200/200?random=3", rot: -4 },
  { src: "https://picsum.photos/200/200?random=4", rot: 8 },
  { src: "https://picsum.photos/200/200?random=5", rot: -6 },
  { src: "https://picsum.photos/200/200?random=6", rot: 3 },
]);

const lyrics = ref([
  { text: "Happy Birthday To You", time: 0 },
  { text: "Happy Birthday To You", time: 2.5 },
  { text: "Happy Birthday My Dear Love", time: 5 },
  { text: "Happy Birthday To You", time: 8 },
]);

const audioRef = ref(null);
const gridRef = ref(null);
const lyricRef = ref(null);
const confettiRef = ref(null);
const currentIndex = ref(0);
const needsInteraction = ref(false);
const letters = ref(lyrics.value[0].text.split(""));

function updateLyric() {
  if (!audioRef.value) return;
  const t = audioRef.value.currentTime;
  let idx = 0;
  for (let i = 0; i < lyrics.value.length; i++) {
    if (t >= lyrics.value[i].time) idx = i;
  }
  currentIndex.value = idx;
}

function handleLoop() {
  if (!audioRef.value) return;
  audioRef.value.currentTime = 0;
  audioRef.value.play();
}

async function tryAutoplay() {
  try {
    if (!audioRef.value) return;
    await audioRef.value.play();
    needsInteraction.value = false;
  } catch {
    needsInteraction.value = true;
  }
}

function startPlayback() {
  if (!audioRef.value) return;
  audioRef.value.play();
  needsInteraction.value = false;
}

function animateGrid() {
  if (!gridRef.value) return;
  const cards = gridRef.value.querySelectorAll(".pic-card");
  animate(cards, {
    opacity: [0, 1],
    scale: [0.3, 1],
    translateY: [40, 0],
    rotate: (el, i) => imgs.value[i].rot,
    delay: stagger(130, { start: 250 }),
    duration: 900,
    easing: "easeOutElastic(1, .6)",
  });
}

function onCardEnter(e, rot) {
  animate(e.currentTarget, {
    rotate: 0,
    scale: 1.12,
    duration: 350,
    easing: "easeOutQuad",
  });
}

function onCardLeave(e, rot) {
  animate(e.currentTarget, {
    rotate: rot,
    scale: 1,
    duration: 450,
    easing: "easeOutElastic(1, .6)",
  });
}

function animateFloaters() {
  if (!confettiRef.value) return;
  const dots = confettiRef.value.querySelectorAll(".confetto");
  dots.forEach((dot, i) => {
    animate(dot, {
      translateY: [0, -18 - utils.random(0, 20)],
      translateX: [0, (utils.random(0, 100) - 50) * 0.48],
      rotate: [0, (utils.random(0, 100) - 50) * 1.2],
      duration: 2200 + utils.random(0, 1400),
      delay: i * 180,
      direction: "alternate",
      loop: true,
      easing: "easeInOutSine",
    });
  });
}

async function animateLyric() {
  await nextTick();
  if (!lyricRef.value) return;
  const spans = lyricRef.value.querySelectorAll(".letter");
  animate(spans, {
    opacity: [0, 1],
    translateY: [24, 0],
    rotateZ: [() => utils.random(-12, 12), 0],
    scale: [0.6, 1],
    delay: stagger(28),
    duration: 650,
    easing: "easeOutBack",
  });
}

watch(currentIndex, (idx) => {
  letters.value = lyrics.value[idx].text.split("");
  animateLyric();
});

onMounted(() => {
  tryAutoplay();
  animateGrid();
  animateFloaters();
  animateLyric();
});
</script>

<template>
  <div class="page">
    <audio
      ref="audioRef"
      src="/song.mp3"
      loop
      @timeupdate="updateLyric"
      @ended="handleLoop"
    ></audio>

    <button v-if="needsInteraction" class="play-btn" @click="startPlayback">
      ▶ Tap to play
    </button>


    <h1 class="title">
      <span class="title-cake">🎂</span> Happy Birthday
      <span class="title-cake">🎉</span>
    </h1>

    <div class="pic-band" ref="gridRef">
      <div
        v-for="img in imgs"
        :key="img.src"
        class="pic-card"
        @mouseenter="(e) => onCardEnter(e, img.rot)"
        @mouseleave="(e) => onCardLeave(e, img.rot)"
      >
        <img :src="img.src" alt="Memory" />
      </div>
    </div>

    <div class="bday-song">
      <p class="lyric-line" ref="lyricRef" :key="currentIndex">
        <span v-for="(ch, i) in letters" :key="i" class="letter">{{
          ch === " " ? "\u00A0" : ch
        }}</span>
      </p>
    </div>

    <Hearts />
  </div>
</template>

<style scoped>
.page {
  position: relative;
  min-height: 100vh;
  padding: 24px 16px 60px;
  background: radial-gradient(
    circle at 20% 0%,
    #fff2f6 0%,
    #ffe3ec 45%,
    #ffd3e1 100%
  );
  overflow: hidden;
  font-family: "Poppins", "Segoe UI", sans-serif;
}



.title {
  position: relative;
  z-index: 1;
  text-align: center;
  font-size: 1.7rem;
  font-weight: 800;
  margin: 4px 0 8px;
  color: #c23a63;
  letter-spacing: 0.5px;
  text-shadow: 0 2px 0 rgba(255, 255, 255, 0.6);
}
.title-cake {
  display: inline-block;
  filter: drop-shadow(0 2px 3px rgba(0, 0, 0, 0.15));
}

.pic-band {
  position: relative;
  z-index: 1;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 14px;
  max-width: 340px;
  margin: 28px auto 12px;
  padding: 0 6px;
}

.pic-card {
  background: #fff;
  padding: 8px 8px 16px;
  border-radius: 6px;
  box-shadow:
    0 6px 14px rgba(194, 58, 99, 0.18),
    0 1px 2px rgba(0, 0, 0, 0.08);
  will-change: transform;
  cursor: pointer;
}

.pic-card:nth-child(even) {
  margin-top: 18px;
}

.pic-card img {
  display: block;
  width: 100%;
  aspect-ratio: 1 / 1;
  object-fit: cover;
  border-radius: 3px;
}

.bday-song {
  position: relative;
  z-index: 1;
  min-height: 4.5rem;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
  margin-top: 18px;
  padding: 0 12px;
}

.lyric-line {
  margin: 0;
  text-align: center;
  font-family: "Brush Script MT", "Segoe Script", cursive;
  font-size: 2.1rem;
  font-weight: 700;
  line-height: 1.2;
  background: linear-gradient(90deg, #ff5f8f, #ffb648, #ff5f8f);
  background-size: 200% auto;
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
  animation: shimmer 3.5s linear infinite;
}

.letter {
  display: inline-block;
  will-change: transform, opacity;
}

@keyframes shimmer {
  to {
    background-position: 200% center;
  }
}

.play-btn {
  position: fixed;
  top: 1rem;
  right: 1rem;
  padding: 0.5rem 1.1rem;
  border-radius: 999px;
  border: none;
  background: linear-gradient(135deg, #ff5f8f, #ff8fb1);
  color: #fff;
  font-weight: 600;
  box-shadow: 0 4px 10px rgba(255, 95, 143, 0.4);
  cursor: pointer;
  z-index: 10;
}
</style>
