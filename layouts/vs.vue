<script setup lang="ts">
import { computed, useSlots } from 'vue'
import { useSlideContext } from '@slidev/client'
import LayoutBase from '../components/LayoutBase.vue'

const slots = useSlots()
const { $frontmatter: fm } = useSlideContext()

// Fallback to frontmatter if author ported from Tahta
const hasLeftSlot = computed(() => !!slots.left)
const hasRightSlot = computed(() => !!slots.right)

const L = computed(() => fm.left || {})
const R = computed(() => fm.right || {})
const label = computed(() => fm.label || 'VS')
</script>

<template>
  <LayoutBase class="layout-vs">
    <!-- Header title/subtitle from default slot -->
    <div v-if="$slots.default" class="mb-4">
      <slot />
    </div>

    <!-- Comparison Grid -->
    <div class="relative grid grid-cols-2 gap-8 flex-1 min-h-0 items-stretch">
      <!-- Left Column / Option A -->
      <div class="vs-card vs-left flex flex-col min-h-0 bg-[var(--slidev-theme-surface)] border border-[var(--slidev-theme-border)] border-t-4 border-t-[var(--slidev-theme-primary)] rounded-xl p-6 relative overflow-hidden transition-all duration-200">
        <template v-if="hasLeftSlot">
          <slot name="left" />
        </template>
        <template v-else-if="L.title || L.items">
          <h3 v-if="L.title" class="text-xl font-bold mb-4 pb-2 border-b border-[var(--slidev-theme-border)] text-[var(--slidev-theme-primary)]" v-html="L.title" />
          <ul v-if="L.items" class="space-y-2 flex-1">
            <li v-for="(it, i) in L.items" :key="i" v-html="it" />
          </ul>
        </template>
      </div>

      <!-- Center VS Badge -->
      <div class="absolute left-1/2 top-1/2 -translate-x-1/2 -translate-y-1/2 z-20 pointer-events-none flex items-center justify-center">
        <div class="w-11 h-11 rounded-full bg-[var(--slidev-theme-bg)] border-2 border-[var(--slidev-theme-border)] text-[var(--slidev-theme-dim)] font-mono font-extrabold text-sm tracking-wider flex items-center justify-center shadow-lg uppercase select-none">
          {{ label }}
        </div>
      </div>

      <!-- Right Column / Option B (Duo-color Secondary Accent) -->
      <div 
        class="vs-card vs-right flex flex-col min-h-0 bg-[var(--slidev-theme-surface)] border border-[var(--slidev-theme-border)] border-t-4 border-t-[var(--slidev-theme-secondary)] rounded-xl p-6 relative overflow-hidden transition-all duration-200"
        style="--slidev-theme-primary: var(--slidev-theme-secondary);"
      >
        <template v-if="hasRightSlot">
          <slot name="right" />
        </template>
        <template v-else-if="R.title || R.items">
          <h3 v-if="R.title" class="text-xl font-bold mb-4 pb-2 border-b border-[var(--slidev-theme-border)] text-[var(--slidev-theme-secondary)]" v-html="R.title" />
          <ul v-if="R.items" class="space-y-2 flex-1">
            <li v-for="(it, i) in R.items" :key="i" v-html="it" />
          </ul>
        </template>
      </div>
    </div>
  </LayoutBase>
</template>

<style scoped>
.vs-left:hover {
  border-color: color-mix(in srgb, var(--slidev-theme-primary) 40%, var(--slidev-theme-border));
  border-top-color: var(--slidev-theme-primary);
}

.vs-right:hover {
  border-color: color-mix(in srgb, var(--slidev-theme-secondary) 40%, var(--slidev-theme-border));
  border-top-color: var(--slidev-theme-secondary);
}
</style>

