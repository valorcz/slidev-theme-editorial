<script setup lang="ts">
// Technical dot-matrix grid with radial vignette
</script>

<template>
  <div class="absolute inset-0 pointer-events-none overflow-hidden z-0 opacity-30 select-none">
    <div class="tech-grid absolute inset-0"></div>
    <div class="scanner-line absolute left-0 right-0 h-[2px] bg-gradient-to-r from-transparent via-[var(--slidev-theme-primary)] to-transparent opacity-40"></div>
  </div>
</template>

<style scoped>
.tech-grid {
  background-image: radial-gradient(circle at center, var(--slidev-theme-primary) 1.5px, transparent 1.5px);
  background-size: 28px 28px;
  mask-image: radial-gradient(ellipse at 70% 50%, black 25%, transparent 75%);
  -webkit-mask-image: radial-gradient(ellipse at 70% 50%, black 25%, transparent 75%);
}

.scanner-line {
  top: 30%;
  animation: scan 8s ease-in-out infinite alternate;
}

@keyframes scan {
  0% { top: 15%; opacity: 0.2; }
  50% { opacity: 0.5; }
  100% { top: 85%; opacity: 0.15; }
}
</style>
