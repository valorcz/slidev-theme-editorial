<script setup lang="ts">
// Atmospheric ambient glow orbs with slow breathing pulse
</script>

<template>
  <div class="absolute inset-0 pointer-events-none overflow-hidden z-0 select-none">
    <!-- Primary glowing sphere -->
    <div class="glow-orb-primary absolute -top-16 -right-16 w-[480px] h-[480px] rounded-full bg-[var(--slidev-theme-primary)] opacity-15 blur-[120px]"></div>
    <!-- Secondary depth sphere -->
    <div class="glow-orb-secondary absolute -bottom-24 right-1/4 w-[380px] h-[380px] rounded-full bg-[var(--slidev-theme-primary)] opacity-10 blur-[100px]"></div>
  </div>
</template>

<style scoped>
.glow-orb-primary {
  animation: floatOrb 12s ease-in-out infinite alternate;
}

.glow-orb-secondary {
  animation: floatOrb 16s ease-in-out infinite alternate-reverse;
}

@keyframes floatOrb {
  0% { transform: translate(0, 0) scale(1); }
  50% { transform: translate(-30px, 40px) scale(1.1); }
  100% { transform: translate(20px, -20px) scale(0.95); }
}
</style>
