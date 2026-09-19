<script setup lang="ts">
import { computed } from 'vue'
import { useSlideContext } from '@slidev/client'
import LayoutBase from '../components/LayoutBase.vue'

const { $frontmatter: fm } = useSlideContext()

// Support Tahta frontmatter fallback: steps: [{ title, desc, icon }]
const frontmatterSteps = computed(() => fm.steps || [])
</script>

<template>
  <LayoutBase class="layout-steps">
    <!-- Header title/subtitle -->
    <div class="mb-6">
      <slot />
    </div>

    <!-- If frontmatter steps exist -->
    <div v-if="frontmatterSteps.length > 0" class="steps-container flex items-stretch gap-3 flex-1 min-h-0">
      <template v-for="(s, i) in frontmatterSteps" :key="i">
        <div class="step-card flex-1 flex flex-col justify-between p-5 bg-[var(--slidev-theme-surface)] border border-[var(--slidev-theme-border)] rounded-xl transition-all duration-200">
          <div>
            <div class="flex items-center justify-between mb-3">
              <span class="font-mono text-xs font-bold px-2 py-0.5 rounded bg-[var(--slidev-theme-bg)] border border-[var(--slidev-theme-border)] text-[var(--slidev-theme-primary)]">
                0{{ i + 1 }}
              </span>
              <div v-if="s.icon" :class="[s.icon, 'text-xl text-[var(--slidev-theme-primary)]']"></div>
            </div>
            <div class="text-base font-bold text-[var(--slidev-theme-color)] mb-2">
              {{ s.title }}
            </div>
            <div v-if="s.desc" class="text-sm text-[var(--slidev-theme-dim)] leading-relaxed">
              {{ s.desc }}
            </div>
          </div>
        </div>

        <div v-if="i < frontmatterSteps.length - 1" class="step-arrow flex items-center justify-center text-[var(--slidev-theme-dim)] opacity-50 px-1 select-none">
          <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
          </svg>
        </div>
      </template>
    </div>

    <!-- Otherwise render standard markdown list/slots transformed via CSS -->
    <div v-else class="steps-markdown flex-1 min-h-0 flex items-center">
      <slot name="content" />
    </div>
  </LayoutBase>
</template>

<style>
/* Auto-styling standard ordered/unordered lists into horizontal process cards */
.layout-steps .steps-markdown ol,
.layout-steps .steps-markdown ul,
.layout-steps .flex-1 > ol,
.layout-steps .flex-1 > ul {
  display: flex !important;
  flex-direction: row !important;
  align-items: stretch !important;
  gap: 1rem !important;
  width: 100% !important;
  list-style: none !important;
  padding-left: 0 !important;
  margin: 0 !important;
  counter-reset: step-counter !important;
}

.layout-steps .steps-markdown ol > li,
.layout-steps .steps-markdown ul > li,
.layout-steps .flex-1 > ol > li,
.layout-steps .flex-1 > ul > li {
  flex: 1 !important;
  counter-increment: step-counter !important;
  position: relative !important;
  padding: 1.25rem !important;
  background-color: var(--slidev-theme-surface) !important;
  border: 1px solid var(--slidev-theme-border) !important;
  border-radius: 0.75rem !important;
  display: flex !important;
  flex-direction: column !important;
  justify-content: flex-start !important;
  margin-bottom: 0 !important;
}

.layout-steps .steps-markdown ol > li::before,
.layout-steps .flex-1 > ol > li::before {
  content: counter(step-counter, decimal-leading-zero) !important;
  position: static !important;
  display: inline-block !important;
  width: auto !important;
  height: auto !important;
  background-color: transparent !important;
  font-family: var(--slidev-theme-font-mono) !important;
  font-size: 0.8rem !important;
  font-weight: 700 !important;
  color: var(--slidev-theme-primary) !important;
  margin-bottom: 0.5rem !important;
  border-radius: 0 !important;
}

.layout-steps .steps-markdown ul > li::before,
.layout-steps .flex-1 > ul > li::before {
  display: none !important;
}

.layout-steps .steps-markdown ol > li strong,
.layout-steps .steps-markdown ul > li strong,
.layout-steps .flex-1 > ol > li strong,
.layout-steps .flex-1 > ul > li strong {
  display: block;
  font-size: 1.05rem;
  color: var(--slidev-theme-color);
  margin-bottom: 0.25rem;
}
</style>
