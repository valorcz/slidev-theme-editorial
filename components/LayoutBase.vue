<script setup lang="ts">
import { computed } from 'vue'
import { useSlideContext } from '@slidev/client'
import LayoutFooter from './LayoutFooter.vue'

const props = defineProps<{
  class?: string 
}>()

// 1. Swap useNav for useSlideContext to get the localized $page
const { $slidev, $page } = useSlideContext()

const isSection = (s: any) => (s?.meta?.frontmatter?.layout || s?.meta?.layout || s?.layout) === 'section'
const getTitle = (s: any) => s?.meta?.frontmatter?.title || s?.meta?.slide?.title || s?.title || 'Section'

const currentSectionTitle = computed(() => {
  const slides = $slidev.nav.slides
  const idx = $page.value - 1
  if (!slides || idx <= 0 || isSection(slides[idx])) return null

  for (let i = idx - 1; i >= 0; i--) {
    if (isSection(slides[i])) return getTitle(slides[i])
  }
  return null
})
</script>

<template>
  <div class="slidev-layout h-full w-full relative flex flex-col justify-between overflow-hidden" :class="props.class">
    
    <div v-if="currentSectionTitle" 
         class="absolute top-8 left-6 z-0 pointer-events-none select-none">
      <h1 class="whitespace-nowrap text-5xl font-extrabold tracking-tight text-[var(--slidev-theme-color)] opacity-10"
          style="
            font-family: var(--slidev-theme-font-header);
            transform-origin: top left;
            transform: rotate(-90deg) translateX(-100%);
          ">
        {{ currentSectionTitle }}
      </h1>
    </div>

    <div class="flex-1 flex flex-col min-h-0 relative z-10" 
         :class="currentSectionTitle ? 'pl-12' : ''">
      <slot />
    </div>

    <LayoutFooter showTitle showPage />
    
  </div>
</template>