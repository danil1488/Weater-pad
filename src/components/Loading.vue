<template>
  <div class="loading-screen">
    <p>Loading...</p>
  </div>
</template>

<style scoped>
.loading-screen {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: #000;
  color: #fff;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 2em;
  z-index: 9999;
}
</style>
