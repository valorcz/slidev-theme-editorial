<script setup lang="ts">
import { computed } from 'vue'
import { useSlideContext } from '@slidev/client'
import LayoutBase from '../components/LayoutBase.vue'
import SinusWaves from '../components/SinusWaves.vue'

// USE SLIDECONTEXT AS REQUESTED
const { $slidev } = useSlideContext()

const sectionNumber = computed(() => {
  // 1. Get the raw array of slide metadata
  const slides = $slidev.nav.slides
  // 2. Get current 1-based index
  const currentIndex = $slidev.nav.currentPage - 1
  
  let count = 0

  // 3. Loop purely by index (Safe & Robust)
  // We only check slides UP TO the current one.
  for (let i = 0; i <= currentIndex; i++) {
    const slide = slides[i]
    
    // 4. Drill down to find the layout
    const meta = slide.meta || {}
    const frontmatter = meta.frontmatter || (slide as any).frontmatter || {}
    
    const layout = 
      frontmatter.layout || 
      meta.layout || 
      (slide as any).layout

    if (layout === 'section') {
      count++
    }
  }
  
  // Pad with leading zero (01, 02, etc.)
  return count.toString().padStart(2, '0')
})
</script>

<template>
  <LayoutBase class="section relative overflow-hidden">
    
    <SinusWaves />

    <div class="absolute top-0 right-0 p-12 select-none pointer-events-none z-10">
      <span class="text-[10rem] font-black leading-none text-[var(--slidev-theme-primary)] opacity-10 font-mono tracking-tighter">
        {{ sectionNumber }}
      </span>
    </div>

    <div class="h-full w-full flex flex-col justify-center items-start text-left relative z-10 pr-24">
      <div class="w-20 h-2 bg-[var(--slidev-theme-primary)] mb-8 rounded-full"></div>
      <slot />
    </div>

  </LayoutBase>
</template>

<style scoped>
.slidev-layout.section :deep(h1) {
  font-size: 4.5rem;
  line-height: 1;
  font-weight: 800;
  margin-bottom: 1.5rem;
  letter-spacing: -0.02em;
}

.slidev-layout.section :deep(p) {
  font-size: 1.5rem;
  opacity: 0.6;
  font-weight: 400;
  max-width: 800px;
  line-height: 1.4;
}
</style>
