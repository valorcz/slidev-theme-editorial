<script setup lang="ts">
import LayoutBase from '../components/LayoutBase.vue'
</script>

<template>
  <LayoutBase class="end">
    
    <div class="h-full w-full flex flex-col items-center justify-center text-center pb-20">
      
      <div class="text-9xl text-[var(--slidev-theme-primary)] opacity-80 mb-8">
        <span class="i-carbon-chat" style="display: inline-block;"></span>
      </div>

      <div class="max-w-2xl">
        <slot />
      </div>

    </div>

  </LayoutBase>
</template>

<style scoped>
/* Force H1 to be massive and centered 
   Overrides any default left-alignment or margins 
*/
.slidev-layout.end :deep(h1) {
  font-size: 4.5rem !important;
  line-height: 1.1;
  margin-bottom: 1.5rem;
  font-weight: 800;
  letter-spacing: -0.02em;
}

/* Subtext styling */
.slidev-layout.end :deep(p) {
  font-size: 1.5rem;
  opacity: 0.6;
  font-family: var(--slidev-theme-font-mono); /* Technical feel */
}
</style>