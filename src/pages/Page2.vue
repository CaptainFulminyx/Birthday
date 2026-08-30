<script setup>
import { ref, onMounted, onUnmounted } from "vue";

const imgs = ref([
  { src: "https://picsum.photos/200/200?random=1" },
  { src: "https://picsum.photos/200/200?random=2" },
  { src: "https://picsum.photos/200/200?random=3" },
  { src: "https://picsum.photos/200/200?random=4" },
  { src: "https://picsum.photos/200/200?random=5" },
  { src: "https://picsum.photos/200/200?random=6" },
]);

// give each line a start time (in seconds) matching the mp3
const lyrics = ref([
  { text: "Happy Birthday To You", time: 0 },
  { text: "Happy Birthday To You", time: 2.5 },
  { text: "Happy Birthday My Dear Love", time: 5 },
  { text: "Happy Birthday To You", time: 8 },
]);

const audioRef = ref(null);
const currentIndex = ref(0);
const needsInteraction = ref(false);

function updateLyric() {
  const t = audioRef.value.currentTime;
  // find the last lyric whose time has passed
  let idx = 0;
  for (let i = 0; i < lyrics.value.length; i++) {
    if (t >= lyrics.value[i].time) idx = i;
  }
  currentIndex.value = idx;
}

function handleLoop() {
  // if audio ends before lyrics naturally loop, restart both together
  audioRef.value.currentTime = 0;
  audioRef.value.play();
}

async function tryAutoplay() {
  try {
    await audioRef.value.play();
    needsInteraction.value = false;
  } catch (err) {
    // autoplay blocked — wait for user tap
    needsInteraction.value = true;
  }
}

function startPlayback() {
  audioRef.value.play();
  needsInteraction.value = false;
}

onMounted(() => {
  tryAutoplay();
});
</script>

<template>
  <div class="page">
    <audio
      ref="audioRef"
      src="/song.mp3"
      loop
      @timeupdate="updateLyric"
    ></audio>

    <button v-if="needsInteraction" class="play-btn" @click="startPlayback">
      ▶ Tap to play
    </button>

    <div class="pic-band">
      <div v-for="img in imgs" :key="img.src">
        <img :src="img.src" alt="Image" />
      </div>
    </div>

    <div class="bday-song">
      <Transition name="lyric" mode="out-in">
        <p :key="currentIndex" class="lyric-line">
          {{ lyrics[currentIndex].text }}
        </p>
      </Transition>
    </div>
  </div>
</template>

<style scoped>
.page {
  padding: 20px;
}

.pic-band {
  position: relative;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  max-width: 300px;
  margin: 20px auto;
  scale: 0.95;
  gap: 10px;
  background: var(--color-pink-deep);
  transform: rotate(10deg);
  padding: 20px;
  width: 250px;
  height: 400px;
}

/* Base style for both pieces of tape */
.pic-band::before,
.pic-band::after {
  content: "";
  position: absolute;
  width: 100px;
  height: 28px;
  background-color: rgba(255, 100, 200, 0.7);
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.12);
  backdrop-filter: blur(3px);
  z-index: 2;

  /* Jagged, torn cut effect on the left and right ends */
  clip-path: polygon(
    0% 0%,
    5% 35%,
    0% 70%,
    4% 100%,
    96% 100%,
    100% 65%,
    95% 30%,
    100% 0%
  );
}

/* Top-left tape */
.pic-band::before {
  top: -12px;
  left: -30px;
  transform: rotate(-35deg);
}

/* Bottom-right tape */
.pic-band::after {
  bottom: -12px;
  right: -30px;
  transform: rotate(-35deg);
}

.pic-band img {
  width: 120%;
  aspect-ratio: 1 / 1;
  object-fit: cover;
  border: 5px solid #fff;
  border-radius: 20px;
  box-sizing: border-box;
}

.bday-song {
  height: 4rem;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}
.lyric-line {
  font-size: 1.5rem;
  font-weight: 600;
  text-align: center;
  margin: 0;
}
.lyric-enter-active,
.lyric-leave-active {
  transition: all 0.5s ease;
}
.lyric-enter-from {
  opacity: 0;
  transform: translateY(20px);
}
.lyric-leave-to {
  opacity: 0;
  transform: translateY(-20px);
}
.play-btn {
  position: fixed;
  top: 1rem;
  right: 1rem;
  padding: 0.5rem 1rem;
  border-radius: 999px;
  border: none;
  background: #000;
  color: #fff;
  cursor: pointer;
  z-index: 10;
}
</style>
