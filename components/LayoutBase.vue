<script setup lang="ts">
import { computed } from 'vue'
import { useSlideContext } from '@slidev/client'
import LayoutFooter from './LayoutFooter.vue'
import SectionRail from './SectionRail.vue'

const props = withDefaults(
  defineProps<{
    class?: string
    watermark?: boolean | string
    footer?: boolean
    rail?: boolean
    aside?: boolean | string
    kicker?: string
    title?: string
    subtitle?: string
  }>(),
  {
    watermark: true,
    footer: true,
    rail: true,
  }
)

const { $slidev, $page, $frontmatter: fm } = useSlideContext()

const isSection = (s: any) => {
  const frontmatter = s?.meta?.slide?.frontmatter || s?.meta?.frontmatter || (s as any)?.frontmatter || {}
  const layout = frontmatter.layout || s?.meta?.layout || (s as any)?.layout
  return layout === 'section'
}

const getTitle = (s: any) => {
  const frontmatter = s?.meta?.slide?.frontmatter || s?.meta?.frontmatter || (s as any)?.frontmatter || {}
  return frontmatter.title || s?.meta?.slide?.title || s?.title || 'Section'
}

// Compute section title watermark
const activeWatermark = computed(() => {
  if (fm.watermark === false || props.watermark === false) return null
  if (typeof fm.watermark === 'string') return fm.watermark
  if (typeof props.watermark === 'string') return props.watermark

  const slides = $slidev.nav.slides
  const idx = $page.value - 1
  if (!slides || idx <= 0 || isSection(slides[idx])) return null

  for (let i = idx - 1; i >= 0; i--) {
    if (isSection(slides[i])) return getTitle(slides[i])
  }
  return null
})

const showFooter = computed(() => {
  return fm.footer !== false && props.footer !== false
})

const asideLabel = computed(() => {
  const val = fm.aside ?? props.aside
  if (!val) return null
  return typeof val === 'string' ? val : 'deep dive'
})

// Optional header fallback for Tahta migration
const displayKicker = computed(() => fm.kicker || props.kicker)
const displayTitle = computed(() => fm.title || props.title)
const displaySubtitle = computed(() => fm.subtitle || props.subtitle)

// Multi-Accent & ThemeConfig Styles
const slideStyles = computed(() => {
  const styles: Record<string, string> = {}
  const cfg = ($slidev.configs?.themeConfig as any) || {}
  
  if (cfg.primary) styles['--slidev-theme-primary'] = cfg.primary
  if (cfg.secondary) styles['--slidev-secondary-override'] = cfg.secondary
  if (cfg.tertiary) styles['--slidev-tertiary-override'] = cfg.tertiary

  // Per-slide explicit accent override
  if (fm.accent) {
    if (fm.accent === 'secondary') {
      styles['--slidev-theme-primary'] = 'var(--slidev-theme-secondary)'
    } else if (fm.accent === 'tertiary') {
      styles['--slidev-theme-primary'] = 'var(--slidev-theme-tertiary)'
    } else if (typeof fm.accent === 'string') {
      styles['--slidev-theme-primary'] = fm.accent
    }
  } else if (cfg.sectionAccents) {
    // Automatic chapter pacing: section chapters cycle across the categorical ramp
    const slides = $slidev.nav.slides
    const idx = $page.value - 1
    let sCount = 0
    if (slides && idx >= 0) {
      for (let i = 0; i <= idx; i++) {
        if (isSection(slides[i])) sCount++
      }
    }
    if (sCount > 0) {
      const ramps = ['var(--cat-1)', 'var(--cat-2)', 'var(--cat-3)', 'var(--cat-4)', 'var(--cat-5)']
      styles['--slidev-theme-primary'] = ramps[(sCount - 1) % ramps.length]
    }
  }

  return styles
})
</script>

<template>
  <div 
    class="slidev-layout h-full w-full relative flex flex-col justify-between overflow-hidden" 
    :class="[props.class, fm.accent ? `accent-${fm.accent}` : '']"
    :style="slideStyles"
  >
    
    <!-- Top Progress Rail -->
    <SectionRail v-if="props.rail" />


    <!-- Corner Aside Tag (e.g. aside: 'deep dive') -->
    <div v-if="asideLabel" class="aside-tag select-none">
      {{ asideLabel }}
    </div>

    <!-- Rotated Section Title Watermark -->
    <div 
      v-if="activeWatermark" 
      class="watermark-container absolute top-8 left-6 z-0 pointer-events-none select-none"
    >
      <div 
        class="watermark-text whitespace-nowrap text-5xl font-extrabold tracking-tight text-[var(--slidev-theme-color)] opacity-10"
        style="
          font-family: var(--slidev-theme-font-header);
          font-size: 3rem !important;
          line-height: 1 !important;
          transform-origin: top left;
          transform: rotate(-90deg) translateX(-100%);
        "
      >
        {{ activeWatermark }}
      </div>
    </div>

    <!-- Main Content Container -->
    <div 
      class="flex-1 flex flex-col min-h-0 relative z-10" 
      :class="activeWatermark ? 'pl-12' : ''"
    >
      <!-- Optional fallback header for decks using frontmatter kicker/title/subtitle -->
      <div v-if="displayKicker || displayTitle" class="mb-4">
        <div v-if="displayKicker" class="kicker">{{ displayKicker }}</div>
        <h1 v-if="displayTitle" v-html="displayTitle" />
        <h3 v-if="displaySubtitle" v-html="displaySubtitle" />
      </div>

      <slot />
    </div>

    <!-- Layout Footer -->
    <LayoutFooter v-if="showFooter" showTitle showPage />
    
  </div>
</template>