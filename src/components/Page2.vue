<script setup>
import { ref } from "vue";

const imgs = ref([
  { src: "https://picsum.photos/200/200?random=1" },
  { src: "https://picsum.photos/200/200?random=2" },
  { src: "https://picsum.photos/200/200?random=3" },
  { src: "https://picsum.photos/200/200?random=4" },
  { src: "https://picsum.photos/200/200?random=5" },
  { src: "https://picsum.photos/200/200?random=6" },
]);
</script>

<template>
  <div class="page">
    <div class="pic-band">
      <div v-for="img in imgs" :key="img.src">
        <img :src="img.src" alt="Image" />
      </div>
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
    /* Jagged left edge */ 96% 100%,
    100% 65%,
    95% 30%,
    100% 0% /* Jagged right edge */
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
</style>
