<script setup lang="ts">
import { computed } from 'vue'
import { useSlideContext } from '@slidev/client'
import LayoutFooter from '../components/LayoutFooter.vue'
import SectionRail from '../components/SectionRail.vue'
import SectionEffect from '../components/SectionEffect.vue'

const { $slidev, $page, $frontmatter: fm } = useSlideContext()

const sectionIndexNumber = computed(() => {
  const slides = $slidev.nav.slides
  const myIndex = $page.value - 1
  let count = 0
  for (let i = 0; i <= myIndex; i++) {
    const slide = slides[i]
    const meta = slide.meta || {}
    const frontmatter = meta.frontmatter || (slide as any).frontmatter || {}
    const layout = frontmatter.layout || meta.layout || (slide as any).layout
    if (layout === 'section') {
      count++
    }
  }
  return count
})

const sectionNumberFormatted = computed(() => {
  return sectionIndexNumber.value.toString().padStart(2, '0')
})

// Watermark controls: can be disabled via watermark: false or number: false
const showWatermark = computed(() => {
  return fm.watermark !== false && fm.number !== false && fm.index !== false
})

const watermarkContent = computed(() => {
  if (typeof fm.watermark === 'string') return fm.watermark
  if (typeof fm.number === 'string' || typeof fm.number === 'number') return String(fm.number)
  return sectionNumberFormatted.value
})

// Full-screen background image & translucency controls
const bgImage = computed(() => fm.image || fm.background || fm.bgImage)
const bgOpacity = computed(() => {
  if (typeof fm.imageOpacity === 'number') return fm.imageOpacity
  if (typeof fm.opacity === 'number') return fm.opacity
  return 0.18 // subtle editorial default
})
const bgBlur = computed(() => {
  if (fm.blur === true) return 'blur(8px)'
  if (typeof fm.blur === 'number') return `blur(${fm.blur}px)`
  return 'none'
})

// Selectable animation effects: 'waves' | 'particles' | 'grid' | 'glow' | 'cube' | 'none' | 'random'
const activeEffect = computed(() => {
  if (fm.effect !== undefined) return fm.effect
  const globalEffect = ($slidev.configs?.themeConfig as any)?.sectionEffect || ($slidev.configs as any)?.sectionEffect
  if (globalEffect) return globalEffect
  if (bgImage.value) return 'none'
  return 'waves'
})
</script>

<template>
  <div class="slidev-layout section h-full w-full relative flex flex-col justify-center px-16 overflow-hidden bg-[var(--slidev-theme-bg)]">
    
    <!-- Top Progress Rail -->
    <SectionRail />

    <!-- Optional Full-screen Background Image with Adjustable Translucency -->
    <div 
      v-if="bgImage"
      class="absolute inset-0 z-0 pointer-events-none bg-cover bg-center transition-opacity duration-300"
      :style="{
        backgroundImage: `url(${bgImage})`,
        opacity: bgOpacity,
        filter: bgBlur,
      }"
    />

    <!-- Subtle gradient scrim for readability when background image is present -->
    <div 
      v-if="bgImage && fm.scrim !== false"
      class="absolute inset-0 z-0 pointer-events-none bg-gradient-to-r from-[var(--slidev-theme-bg)] via-[color-mix(in_srgb,var(--slidev-theme-bg)_85%,transparent)] to-transparent"
    />

    <!-- Selectable Background Animation Effect (waves, grid, glow, cube, none, random) -->
    <SectionEffect :effect="activeEffect" :seed="sectionIndexNumber" />

    <!-- Big Section Number Watermark (can be disabled via watermark: false or number: false) -->
    <div v-if="showWatermark" class="absolute top-6 right-8 select-none pointer-events-none z-10">
      <span class="text-[9rem] font-black leading-none text-[var(--slidev-theme-primary)] opacity-10 font-mono tracking-tighter">
        {{ watermarkContent }}
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
