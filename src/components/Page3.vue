<template>
  <div class="page">
    <div class="pic">
      <img src="../assets/ghostwithcake.jpg" />
    </div>
    <div class="message">
      <h2>
        Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod
        tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim
        veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea
        commodo consequat. Duis aute irure dolor in reprehenderit in voluptate
        velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint
        occaecat cupidatat non proident, sunt in culpa qui officia deserunt
        mollit anim id est laborum.
      </h2>
    </div>
  </div>
</template>
<style scoped>
.page {
  padding: 20px;
  width: 100vw;
  height: 100vh;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.pic {
  width: 300px;
  height: 300px;
  background: var(--color-pink-cotton);
  border: 5px solid var(--color-pink-cherry);
  border-radius: 20px;
  overflow: hidden;
  align-self: flex-start;
}
.message {
  width: 300px;
  height: 300px;
  background: var(--color-pink-cotton);
  border: 5px solid var(--color-pink-cherry);
  border-radius: 20px;
  overflow: hidden;
  align-self: flex-end;
  padding: 15px;
  overflow-y: scroll;
}
</style>
