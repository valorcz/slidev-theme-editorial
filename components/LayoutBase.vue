<script setup lang="ts">
import { computed } from 'vue'
import { useNav } from '@slidev/client'
import LayoutFooter from './LayoutFooter.vue'

const props = defineProps<{
  class?: string // Allow passing extra classes for specific layouts
}>()

const { slides, currentPage } = useNav()

// CENTRALIZED LOGIC: Find the section title
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
  <div class="slidev-layout h-full relative" :class="props.class">
    
    <div v-if="currentSectionTitle" 
         class="absolute top-6 left-6 z-0 pointer-events-none select-none">
      <h1 class="whitespace-nowrap text-6xl font-extrabold tracking-tight text-[var(--slidev-theme-color)] opacity-10"
          style="
            font-family: var(--slidev-theme-font-header);
            transform-origin: top left;
            transform: rotate(-90deg) translateX(-100%);
          ">
        {{ currentSectionTitle }}
      </h1>
    </div>

    <div class="h-full w-full pt-6 pb-4 pr-8 flex flex-col" 
         :class="currentSectionTitle ? 'pl-20' : 'pl-8'">
      <slot />
    </div>

    <LayoutFooter showTitle showPage />
    
  </div>
</template>