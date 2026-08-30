<script setup>
import { onMounted, onUnmounted } from "vue";
import { Icon } from "@iconify/vue";
import { animate, utils } from "animejs";

const setCoords = (i, n) => {
  let w = window.innerWidth;
  let h = window.innerHeight;
  if (i % 2 == 0) {
    return [-70, (h / n) * i, 15];
  } else {
    return [w - 120, (h / n) * i, -15];
  }
};

const icons = Array.from({ length: 4 }, (_, i) => ({
  x: setCoords(i, 4)[0],
  y: setCoords(i, 4)[1],
  r: setCoords(i, 4)[2],
}));

let animations = [];

function floatAround(el, radius = 30) {
  const step = () => {
    const anim = animate(el, {
      translateX: utils.random(-radius, radius),
      translateY: utils.random(-radius, radius),
      duration: utils.random(5000, 5000),
      ease: "inOutSine",
      onComplete: step,
    });
    animations.push(anim);
  };
  step();
}

onMounted(() => {
  document.querySelectorAll(".hearts .icon").forEach((el) => {
    floatAround(el);
  });
});

onUnmounted(() => {
  animations.forEach((a) => a.revert());
});
</script>
<template>
  <div class="hearts">
    <Icon
      v-for="(icon, index) in icons"
      :key="index"
      icon="line-md:heart-filled"
      class="icon h-50 w-50 text-pink-deep absolute"
      :style="{
        position: 'absolute',
        opacity: '0.3',
        left: `${icon.x}px`,
        top: `${icon.y}px`,
        transform: `rotate(${icon.r}deg)`,
      }"
    />
  </div>
</template>

<style scoped>
.heart {
  color: #e8477a;
}
</style>
