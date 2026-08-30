<script setup>
import Hearts from "$/Hearts.vue";
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
    <Hearts />
    <div
      style="
        border-radius: 20px;
        border: 5px solid #fb5092;
        overflow: hidden;
        width: 350px;
        height: 350px;
      "
    >
      <h3 style="position: absolute; right: 20px; margin-right: 20px">
        Swipe >>>
      </h3>
      <img src="../assets/ghostwithcake.jpg" style="width: 100%" />
    </div>
    <h1
      class="heading"
      style="
        font-size: 4.2rem;
        text-align: center;
        margin-left: -50px;
        margin-top: -50px;
        line-height: 0.95;
      "
    >
      Happy <br />
      Birthday
    </h1>
  </div>
</template>

<style scoped>
.page {
  max-width: 100vw;
  max-height: 100vh;
  margin: 0 auto;
  padding: 2.5rem 2rem 3rem 2rem;
  display: flex;
  flex-direction: column;
  align-items: center;
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

@media (max-width: 400px) {
  .heading {
    font-size: 2.2rem;
  }
  .message {
    font-size: 1rem;
  }
}
</style>
