<script setup lang="ts">
import { computed } from 'vue'
import { useSlideContext } from '@slidev/client'
import LayoutBase from '../components/LayoutBase.vue'

const { $frontmatter: fm } = useSlideContext()

const term = computed(() => fm.term)
const termType = computed(() => fm.type || fm.partOfSpeech)
const definition = computed(() => fm.definition)
const points = computed<string[]>(() => fm.points || [])
</script>

<template>
  <LayoutBase class="layout-define">
    <div class="my-auto max-w-4xl flex flex-col justify-center">
      
      <!-- Primary Mode: Slotted Markdown Content -->
      <template v-if="$slots.default">
        <slot />
      </template>

      <!-- Fallback Mode: Frontmatter Bridge -->
      <template v-else-if="term">
        <div class="border-b border-[var(--slidev-theme-border)] pb-6 mb-6">
          <div class="flex items-baseline gap-4 flex-wrap">
            <h1 class="text-5xl md:text-6xl font-extrabold text-[var(--slidev-theme-color)] !mb-0 tracking-tight">
              {{ term }}
            </h1>
            <span v-if="termType" class="font-mono text-sm uppercase tracking-widest text-[var(--slidev-theme-primary)] opacity-85">
              {{ termType }}
            </span>
          </div>
        </div>

        <div v-if="definition" class="text-xl md:text-2xl text-[var(--slidev-theme-dim)] leading-relaxed mb-6 font-light">
          {{ definition }}
        </div>

        <ul v-if="points.length > 0" class="space-y-2.5">
          <li v-for="(p, i) in points" :key="i" class="text-base text-[var(--slidev-theme-color)] opacity-90" v-html="p" />
        </ul>
      </template>

    </div>
  </LayoutBase>
</template>

<style>
/* Auto-styling standard Markdown for layout-define */
.layout-define .my-auto h1 {
  font-size: 3.5rem !important;
  margin-bottom: 0.25rem !important;
}

.layout-define .my-auto h4 {
  margin-bottom: 1.5rem !important;
  padding-bottom: 1rem !important;
  border-bottom: 1px solid var(--slidev-theme-border) !important;
  font-size: 0.85rem !important;
}

.layout-define .my-auto > p:first-of-type {
  font-size: 1.4rem !important;
  line-height: 1.5 !important;
  opacity: 0.85 !important;
  margin-bottom: 1.5rem !important;
}
</style>
