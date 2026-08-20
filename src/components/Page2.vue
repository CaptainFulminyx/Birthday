<script setup>
import { ref } from "vue";

// Array of image sources
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
      <!-- Fixed: Added the missing </div> -->
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
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  max-width: 300px;
  margin: 0 auto;
  scale: 0.65;
  gap: 10px;
  background: var(--color-pink-deep);
  transform: rotate(10deg);
  padding: 20px;
}

.pic-band img {
  width: 100%;
  height: 200px;

  border: 10px solid #fff;
  border-radius: 20px;
}
</style>
