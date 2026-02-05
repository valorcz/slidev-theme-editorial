<script setup lang="ts">
import { computed } from 'vue'
import { useNav } from '@slidev/client'
import LayoutFooter from '../components/LayoutFooter.vue'

const { slides, currentPage } = useNav()

const currentSectionTitle = computed(() => {
  const idx = currentPage.value - 1
  const allSlides = slides.value
  
  if (!allSlides || idx < 0) return null

  for (let i = idx; i >= 0; i--) {
    const slide = allSlides[i]
    const meta = slide.meta || {}
    const frontmatter = meta.frontmatter || (slide as any).frontmatter || {}
    const layout = frontmatter.layout || meta.layout || (slide as any).layout

    if (layout === 'section') {
      return frontmatter.title || meta.slide?.title || slide.title || 'Section'
    }
  }
  return null
})
</script>

<template>
  <div class="slidev-layout quote h-full w-full relative overflow-hidden">
    
    <div v-if="currentSectionTitle" 
         class="absolute top-6 right-6 z-0 pointer-events-none select-none">
      <h1 class="whitespace-nowrap text-6xl font-extrabold tracking-tight text-[var(--slidev-theme-color)] opacity-10"
          style="
            font-family: var(--slidev-theme-font-header);
            transform-origin: top right;
            transform: rotate(90deg) translateX(100%);
          ">
        {{ currentSectionTitle }}
      </h1>
    </div>

    <div class="flex flex-col justify-center h-full px-14 relative z-10"
         :class="currentSectionTitle ? 'pr-24' : ''">
      
      <div class="mb-8 relative">
        <div class="w-20 h-2 bg-[var(--slidev-theme-primary)] mb-4 rounded-full"></div>
        <div class="text-8xl text-[var(--slidev-theme-primary)] opacity-40 font-black leading-none select-none font-sans">
          “
        </div>
      </div>

      <blockquote class="relative border-none p-0 m-0">
        <h1 class="quote-text text-5xl md:text-6xl lg:text-7xl font-extrabold !leading-[1.1] text-[var(--slidev-theme-color)] tracking-tight mb-8 not-italic"
            style="font-family: var(--slidev-theme-font-header);">
          <slot />
        </h1>
      </blockquote>

      <div class="flex items-center gap-4 mt-4">
        <div class="h-px w-12 bg-[var(--slidev-theme-dim)] opacity-50"></div>
        <span class="font-mono text-sm tracking-[0.2em] uppercase text-[var(--slidev-theme-dim)]">
          <slot name="author" />
        </span>
      </div>

    </div>

    <LayoutFooter :showTitle="false" showPage />

  </div>
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
