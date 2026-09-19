<script setup lang="ts">
import { computed, useSlots } from 'vue'
import { useSlideContext } from '@slidev/client'
import LayoutBase from '../components/LayoutBase.vue'

interface PanelItem {
  title: string
  icon?: string
  items?: string[]
  body?: string
}

const slots = useSlots()
const { $frontmatter: fm } = useSlideContext()

const panels = computed<PanelItem[]>(() => fm.panels || [])

const activePanelsCount = computed(() => {
  if (panels.value.length > 0) return panels.value.length
  let count = 0
  if (slots.one) count++
  if (slots.two) count++
  if (slots.three) count++
  if (slots.four) count++
  return count || 2
})

const gridColsClass = computed(() => {
  const c = activePanelsCount.value
  if (c === 2) return 'grid-cols-1 md:grid-cols-2'
  if (c === 3) return 'grid-cols-1 md:grid-cols-3'
  return 'grid-cols-1 md:grid-cols-2 lg:grid-cols-2' // 4 panels -> 2x2
})
</script>

<template>
  <LayoutBase class="layout-panels">
    <!-- Header title/subtitle from default slot -->
    <div v-if="$slots.default" class="mb-4">
      <slot />
    </div>

    <!-- Slotted mode (named slots ::one::, ::two::, etc.) -->
    <div 
      v-if="$slots.one || $slots.two" 
      class="panels-grid grid gap-5 flex-1 min-h-0 items-stretch"
      :class="gridColsClass"
    >
      <div v-if="$slots.one" class="panel-card p-5 rounded-xl border border-[var(--slidev-theme-border)] bg-[var(--slidev-theme-surface)] flex flex-col min-h-0 overflow-y-auto shadow-xs">
        <slot name="one" />
      </div>
      <div v-if="$slots.two" class="panel-card p-5 rounded-xl border border-[var(--slidev-theme-border)] bg-[var(--slidev-theme-surface)] flex flex-col min-h-0 overflow-y-auto shadow-xs">
        <slot name="two" />
      </div>
      <div v-if="$slots.three" class="panel-card p-5 rounded-xl border border-[var(--slidev-theme-border)] bg-[var(--slidev-theme-surface)] flex flex-col min-h-0 overflow-y-auto shadow-xs">
        <slot name="three" />
      </div>
      <div v-if="$slots.four" class="panel-card p-5 rounded-xl border border-[var(--slidev-theme-border)] bg-[var(--slidev-theme-surface)] flex flex-col min-h-0 overflow-y-auto shadow-xs">
        <slot name="four" />
      </div>
    </div>

    <!-- Frontmatter fallback mode -->
    <div 
      v-else-if="panels.length > 0"
      class="panels-grid grid gap-5 flex-1 min-h-0 items-stretch"
      :class="gridColsClass"
    >
      <div 
        v-for="(p, i) in panels" 
        :key="i"
        class="panel-card p-5 rounded-xl border border-[var(--slidev-theme-border)] bg-[var(--slidev-theme-surface)] flex flex-col min-h-0 overflow-y-auto shadow-xs"
      >
        <div class="flex items-center gap-2.5 mb-3 border-b border-[var(--slidev-theme-border)] pb-2.5">
          <div v-if="p.icon" :class="[p.icon, 'text-xl']" :style="{ color: `var(--cat-${(i % 5) + 1})` }"></div>
          <h3 class="!m-0 text-lg font-bold text-[var(--slidev-theme-color)]" v-html="p.title" />
        </div>
        <ul v-if="Array.isArray(p.items)" class="space-y-1.5 flex-1">
          <li v-for="(it, j) in p.items" :key="j" v-html="it" />
        </ul>
        <div v-else-if="p.body" class="text-sm leading-relaxed text-[var(--slidev-theme-dim)]" v-html="p.body" />
      </div>
    </div>

    <!-- Fallback content slot -->
    <div v-else class="panels-fallback flex-1 min-h-0">
      <slot name="content" />
    </div>
  </LayoutBase>
</template>

<style scoped>
.panel-card:nth-child(5n+1) { --card-accent: var(--cat-1); }
.panel-card:nth-child(5n+2) { --card-accent: var(--cat-2); }
.panel-card:nth-child(5n+3) { --card-accent: var(--cat-3); }
.panel-card:nth-child(5n+4) { --card-accent: var(--cat-4); }
.panel-card:nth-child(5n+5) { --card-accent: var(--cat-5); }

.panel-card {
  position: relative;
  border-top: 3px solid var(--card-accent, var(--slidev-theme-primary));
}

.panel-card:hover {
  border-color: color-mix(in srgb, var(--card-accent, var(--slidev-theme-primary)) 40%, var(--slidev-theme-border));
  border-top-color: var(--card-accent, var(--slidev-theme-primary));
}
</style>

