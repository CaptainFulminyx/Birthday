<script setup>
import { Icon } from "@iconify/vue";

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
        baby girl <3
      </p>
    </div>

    <div class="hearts">
      <Icon
        v-for="(icon, index) in icons"
        :key="index"
        icon="line-md:heart-filled"
        class="h-50 w-50 text-pink-deep absolute"
        :style="{
          position: 'absolute',
          opacity: '0.3',
          left: `${icon.x}px`,
          top: `${icon.y}px`,
          transform: `rotate(${icon.r}deg)`,
        }"
      />
    </div>
    <img src="../assets/ghostwithcake.jpg" style="border-radius: 20px" />
  </div>
</template>

<style scoped>
.page {
  max-width: 100vw;
  margin: 0 auto;
  padding: 2.5rem 2rem 3rem 2rem;
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

.heart {
  color: #e8477a;
}

@media (max-width: 400px) {
  .heading {
    font-size: 2.2rem;
  }
  .message {
    font-size: 1rem;
  }
}
</style>
