<script setup lang="ts">
import LayoutBase from '../components/LayoutBase.vue'
</script>

<template>
  <LayoutBase class="quote">
    <div class="flex flex-col justify-center h-full max-w-4xl mx-auto my-auto relative z-10">
      
      <div class="mb-6 relative">
        <div class="w-16 h-1.5 bg-[var(--slidev-theme-primary)] mb-4 rounded-full"></div>
        <div class="text-7xl md:text-8xl text-[var(--slidev-theme-primary)] opacity-40 font-black leading-none select-none font-sans">
          “
        </div>
      </div>

      <blockquote class="relative border-none p-0 m-0">
        <h1 class="quote-text text-4xl md:text-5xl lg:text-6xl font-extrabold !leading-[1.15] text-[var(--slidev-theme-color)] tracking-tight mb-6 not-italic"
            style="font-family: var(--slidev-theme-font-header);">
          <slot />
        </h1>
      </blockquote>

      <div v-if="$slots.author" class="flex items-center gap-4 mt-2">
        <div class="h-px w-12 bg-[var(--slidev-theme-dim)] opacity-50"></div>
        <span class="font-mono text-xs md:text-sm tracking-[0.2em] uppercase text-[var(--slidev-theme-dim)]">
          <slot name="author" />
        </span>
      </div>

    </div>
  </LayoutBase>
</template>

<style scoped>
/* Inherit Styles */
.quote-text :deep(p) {
  margin: 0;
  display: inline;
  font-size: inherit;
  font-weight: inherit;
  line-height: inherit;
  color: inherit;
  font-family: inherit;
  font-style: normal;
}

/* Dense Mode Override */
.slidev-layout.quote.dense .quote-text {
  font-size: 2rem !important;
  line-height: 1.2 !important;
  margin-bottom: 1.5rem !important;
}

@media (max-width: 640px) {
  .slidev-layout.quote.dense .quote-text {
    font-size: 2rem !important;
  }
}
</style>
