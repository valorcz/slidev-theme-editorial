<script setup lang="ts">
// No imports needed!
defineProps<{
  size?: string // Optional size prop (default: 200px)
}>()
</script>

<template>
  <div class="scene" :style="{ width: size || '200px', height: size || '200px' }">
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
  perspective: 800px; /* Determines 3D depth perception */
  display: inline-block;
}

.cube {
  width: 100%;
  height: 100%;
  position: relative;
  transform-style: preserve-3d;
  transform: rotateX(-30deg) rotateY(-45deg); /* Initial tilt */
  animation: spin 15s infinite linear;
}

/* Common styles for all faces */
.face {
  position: absolute;
  width: 100%;
  height: 100%;
  /* The "Wireframe" Look */
  border: 2px solid var(--slidev-theme-primary); 
  background: rgba(255, 255, 255, 0.02); /* Slight fill for volume */
  box-sizing: border-box;
}

/* Positioning the faces in 3D space 
   Calculation: translateZ = width / 2 
   Since default is 200px, we translate 100px.
   Using CSS calc() makes it dynamic if we change width.
*/
.front  { transform: rotateY(0deg) translateZ(100px); }
.back   { transform: rotateY(180deg) translateZ(100px); }
.right  { transform: rotateY(90deg) translateZ(100px); }
.left   { transform: rotateY(-90deg) translateZ(100px); }
.top    { transform: rotateX(90deg) translateZ(100px); }
.bottom { transform: rotateX(-90deg) translateZ(100px); }

/* Animation Loop */
@keyframes spin {
  0% { transform: rotateX(0deg) rotateY(0deg); }
  100% { transform: rotateX(360deg) rotateY(360deg); }
}
</style>