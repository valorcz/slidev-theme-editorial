<script setup lang="ts">
import { computed } from 'vue'
import { useSlideContext } from '@slidev/client'
import LayoutBase from '../components/LayoutBase.vue'

interface FeatureItem {
  icon?: string
  title: string
  desc: string
}

const { $frontmatter: fm } = useSlideContext()

const features = computed<FeatureItem[]>(() => fm.features || [])
const cols = computed(() => {
  if (fm.columns) return Number(fm.columns)
  const count = features.value.length
  if (count <= 2) return 2
  if (count === 3) return 3
  return 4
})
</script>

<template>
  <LayoutBase class="layout-feature">
    <!-- Header title/subtitle from default slot -->
    <div v-if="$slots.default" class="mb-6">
      <slot />
    </div>

    <!-- Frontmatter features fallback -->
    <div 
      v-if="features.length > 0" 
      class="feature-grid grid gap-5 flex-1 min-h-0 items-stretch"
      :style="{ gridTemplateColumns: `repeat(${cols}, minmax(0, 1fr))` }"
    >
      <div 
        v-for="(f, i) in features" 
        :key="i"
        class="feature-card flex flex-col p-6 rounded-xl border border-[var(--slidev-theme-border)] bg-[var(--slidev-theme-surface)] transition-all duration-200 shadow-xs"
      >
        <div v-if="f.icon" :class="[f.icon, 'text-3xl mb-4']" :style="{ color: `var(--cat-${(i % 5) + 1})` }"></div>
        <div class="text-lg font-bold text-[var(--slidev-theme-color)] mb-2" v-html="f.title" />
        <div class="text-sm text-[var(--slidev-theme-dim)] leading-relaxed flex-1" v-html="f.desc" />
      </div>
    </div>

    <!-- Slotted feature cards / components -->
    <div v-else class="feature-slot-container flex-1 min-h-0 flex flex-col justify-center">
      <slot name="content" />
    </div>
  </LayoutBase>
</template>

<style>
/* Multi-accent card styling */
.layout-feature .feature-card:nth-child(5n+1) { --card-accent: var(--cat-1); }
.layout-feature .feature-card:nth-child(5n+2) { --card-accent: var(--cat-2); }
.layout-feature .feature-card:nth-child(5n+3) { --card-accent: var(--cat-3); }
.layout-feature .feature-card:nth-child(5n+4) { --card-accent: var(--cat-4); }
.layout-feature .feature-card:nth-child(5n+5) { --card-accent: var(--cat-5); }

.layout-feature .feature-card {
  border-top: 3px solid var(--card-accent, var(--slidev-theme-primary));
}

.layout-feature .feature-card:hover {
  border-color: color-mix(in srgb, var(--card-accent, var(--slidev-theme-primary)) 40%, var(--slidev-theme-border));
  border-top-color: var(--card-accent, var(--slidev-theme-primary));
}

/* Auto-styling grid of cards or list if placed in content slot */

.layout-feature .feature-slot-container > ul {
  display: grid !important;
  grid-template-columns: repeat(auto-fit, minmax(240px, 1fr)) !important;
  gap: 1.25rem !important;
  list-style: none !important;
  padding-left: 0 !important;
  margin: 0 !important;
}

.layout-feature .feature-slot-container > ul > li {
  padding: 1.25rem !important;
  background-color: var(--slidev-theme-surface) !important;
  border: 1px solid var(--slidev-theme-border) !important;
  border-radius: 0.75rem !important;
  display: flex !important;
  flex-direction: column !important;
  margin-bottom: 0 !important;
}

.layout-feature .feature-slot-container > ul > li::before {
  display: none !important;
}

.layout-feature .feature-slot-container > ul > li strong {
  display: block;
  font-size: 1.1rem;
  color: var(--slidev-theme-color);
  margin-bottom: 0.35rem;
}
</style>
