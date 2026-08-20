<script setup lang="ts">
import { computed } from 'vue'
import { useSlideContext } from '@slidev/client'
import LayoutFooter from '../components/LayoutFooter.vue'
import SinusWaves from '../components/SinusWaves.vue'

// 1. Destructure $page alongside $slidev
const { $slidev, $page } = useSlideContext()

const sectionNumber = computed(() => {
  const slides = $slidev.nav.slides
  
  // 2. Use $page.value instead of the global navigation state
  const myIndex = $page.value - 1
  
  let count = 0

  // 3. Loop purely up to THIS slide's index
  for (let i = 0; i <= myIndex; i++) {
    const slide = slides[i]
    
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
  
  return count.toString().padStart(2, '0')
})
</script>

<template>
  <div class="slidev-layout section h-full w-full relative flex flex-col justify-center px-16 overflow-hidden bg-[var(--slidev-theme-bg)]">
    
    <!-- Full-bleed background waves -->
    <SinusWaves />

    <!-- Big Section Number -->
    <div class="absolute top-6 right-8 select-none pointer-events-none z-10">
      <span class="text-[9rem] font-black leading-none text-[var(--slidev-theme-primary)] opacity-10 font-mono tracking-tighter">
        {{ sectionNumber }}
      </span>
    </div>

    <!-- Main Content -->
    <div class="relative z-10 max-w-4xl">
      <div class="w-16 h-1.5 bg-[var(--slidev-theme-primary)] mb-6 rounded-full"></div>
      <slot />
    </div>

    <LayoutFooter :showTitle="true" :showPage="true" />
  </div>
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
