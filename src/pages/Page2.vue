<script setup>
import { onMounted, ref } from "vue";
import { animate, stagger } from "animejs";
import Hearts from "$/Hearts.vue";

const imgs = ref([
  { src: "img2.jpg", rot: -7 },
  { src: "img3.jpg", rot: 5 },
  { src: "img4.jpg", rot: -4 },
  { src: "img5.jpg", rot: 8 },
  { src: "img6.jpg", rot: -6 },
  { src: "img7.jpg", rot: 3 },
]);

const gridRef = ref(null);

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

function onCardEnter(e) {
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

onMounted(() => {
  animateGrid();
});
</script>

<template>
  <div class="page">
    <h1 class="title">
      <span class="title-cake">🎂</span> Happy Birthday
      <span class="title-cake">🎉</span>
    </h1>

    <div class="pic-band" ref="gridRef">
      <div
        v-for="img in imgs"
        :key="img.src"
        class="pic-card"
        @mouseenter="(e) => onCardEnter(e)"
        @mouseleave="(e) => onCardLeave(e, img.rot)"
      >
        <img :src="img.src" alt="Memory" />
      </div>
    </div>

    <!-- Empty container reserved for your new feature -->
    <div class="custom-content-slot">
      <!-- Your new content goes here -->
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
  will-change: transform;
  cursor: pointer;
  border: 2px solid var(--color-pink-deep, #ff5f8f);
}

.pic-card img {
  display: block;
  width: 100%;
  aspect-ratio: 1 / 1;
  object-fit: cover;
  border-radius: 3px;
}

.custom-content-slot {
  position: relative;
  z-index: 1;
  min-height: 5rem;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-top: 18px;
  padding: 0 12px;
}
</style>
