<script setup lang="ts">
import { computed } from 'vue'
import { useSlideContext } from '@slidev/client'
import LayoutBase from '../components/LayoutBase.vue'

const { $frontmatter: fm } = useSlideContext()

// Support Tahta frontmatter fallback: items: [{ topic, desc }]
const frontmatterItems = computed(() => fm.items || [])
const currentStep = computed(() => fm.active != null ? Number(fm.active) : null)

const pad = (n: number) => String(n).padStart(2, '0')
</script>

<template>
  <LayoutBase class="layout-agenda">
    <!-- Header title/subtitle -->
    <div class="mb-4">
      <slot />
    </div>

    <!-- If frontmatter items exist, render them directly in editorial card grid -->
    <div v-if="frontmatterItems.length > 0" class="agenda-grid grid grid-cols-1 md:grid-cols-2 gap-4 flex-1 min-h-0">
      <div 
        v-for="(it, i) in frontmatterItems" 
        :key="i"
        class="agenda-card flex items-start gap-4 p-4 rounded-xl border transition-all duration-200"
        :class="[
          currentStep !== null && (i + 1) === currentStep
            ? 'border-[var(--slidev-theme-primary)] bg-[var(--slidev-theme-surface)] shadow-md'
            : currentStep !== null
              ? 'opacity-40 border-[var(--slidev-theme-border)] bg-transparent'
              : 'border-[var(--slidev-theme-border)] bg-[var(--slidev-theme-surface)]'
        ]"
      >
        <span class="font-mono text-2xl font-black text-[var(--slidev-theme-primary)] opacity-80 leading-none pt-0.5 select-none">
          {{ pad(i + 1) }}
        </span>
        <div class="flex-1 min-w-0">
          <div class="font-bold text-lg text-[var(--slidev-theme-color)]" v-html="it.topic" />
          <div v-if="it.desc" class="text-sm text-[var(--slidev-theme-dim)] mt-1 leading-snug" v-html="it.desc" />
        </div>
      </div>
    </div>

    <!-- Otherwise render standard markdown list transformed via CSS -->
    <div v-else class="agenda-markdown-content flex-1 min-h-0">
      <slot name="content" />
    </div>
  </LayoutBase>
</template>

<style>
/* Auto-styling standard ordered lists inside layout-agenda into stylish cards */
.layout-agenda .agenda-markdown-content ol,
.layout-agenda .flex-1 > ol {
  display: grid !important;
  grid-template-columns: repeat(2, minmax(0, 1fr)) !important;
  gap: 1rem !important;
  counter-reset: agenda-counter !important;
  list-style: none !important;
  padding-left: 0 !important;
  margin: 0 !important;
}

.layout-agenda .agenda-markdown-content ol > li,
.layout-agenda .flex-1 > ol > li {
  position: relative !important;
  counter-increment: agenda-counter !important;
  padding: 1.15rem 1.15rem 1.15rem 4rem !important;
  background-color: var(--slidev-theme-surface) !important;
  border: 1px solid var(--slidev-theme-border) !important;
  border-radius: 0.75rem !important;
  font-size: 1.05rem !important;
  line-height: 1.4 !important;
  margin-bottom: 0 !important;
  transition: border-color 0.2s ease, box-shadow 0.2s ease !important;
}

.layout-agenda .agenda-markdown-content ol > li:hover,
.layout-agenda .flex-1 > ol > li:hover {
  border-color: color-mix(in srgb, var(--slidev-theme-primary) 40%, var(--slidev-theme-border)) !important;
}

/* Big editorial number counter */
.layout-agenda .agenda-markdown-content ol > li::before,
.layout-agenda .flex-1 > ol > li::before {
  content: counter(agenda-counter, decimal-leading-zero) !important;
  position: absolute !important;
  left: 1.15rem !important;
  top: 1.15rem !important;
  font-family: var(--slidev-theme-font-mono) !important;
  font-size: 1.6rem !important;
  font-weight: 800 !important;
  color: var(--slidev-theme-primary) !important;
  opacity: 0.85 !important;
  width: auto !important;
  height: auto !important;
  border-radius: 0 !important;
  background-color: transparent !important;
  line-height: 1 !important;
}

.layout-agenda .agenda-markdown-content ol > li strong,
.layout-agenda .flex-1 > ol > li strong {
  display: block;
  font-size: 1.1rem;
  margin-bottom: 0.25rem;
  color: var(--slidev-theme-color);
}
</style>
