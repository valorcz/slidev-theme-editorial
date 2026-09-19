<script setup lang="ts">
// No imports needed!
defineProps<{
  size?: string // Optional size prop (default: 200px)
}>()
</script>

<template>
  <div class="scene" :style="{ '--cube-size': size || '200px' }">
    <div class="cube">
      <div class="face front"></div>
      <div class="face back"></div>
      <div class="face right"></div>
      <div class="face left"></div>
      <div class="face top"></div>
      <div class="face bottom"></div>
    </div>
  </div>
</template>

<style scoped>
.scene {
  width: var(--cube-size);
  height: var(--cube-size);
  perspective: calc(var(--cube-size) * 4);
  display: inline-block;
}

.cube {
  width: 100%;
  height: 100%;
  position: relative;
  transform-style: preserve-3d;
  transform: rotateX(-30deg) rotateY(-45deg);
  animation: spin 15s infinite linear;
}

/* Common styles for all faces */
.face {
  position: absolute;
  width: 100%;
  height: 100%;
  border: 2px solid var(--slidev-theme-primary); 
  background: rgba(255, 255, 255, 0.03);
  box-sizing: border-box;
  backface-visibility: visible;
}

/* Positioning the faces dynamically in 3D space */
.front  { transform: rotateY(0deg) translateZ(calc(var(--cube-size) / 2)); }
.back   { transform: rotateY(180deg) translateZ(calc(var(--cube-size) / 2)); }
.right  { transform: rotateY(90deg) translateZ(calc(var(--cube-size) / 2)); }
.left   { transform: rotateY(-90deg) translateZ(calc(var(--cube-size) / 2)); }
.top    { transform: rotateX(90deg) translateZ(calc(var(--cube-size) / 2)); }
.bottom { transform: rotateX(-90deg) translateZ(calc(var(--cube-size) / 2)); }

/* Animation Loop */
@keyframes spin {
  0% { transform: rotateX(0deg) rotateY(0deg); }
  100% { transform: rotateX(360deg) rotateY(360deg); }
}
</style>