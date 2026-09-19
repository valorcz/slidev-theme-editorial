<script setup lang="ts">
import { computed } from 'vue'
import { useSlideContext } from '@slidev/client'
import LayoutBase from '../components/LayoutBase.vue'

const props = defineProps<{
  value?: string | number
  unit?: string
  label?: string
  kicker?: string
  tone?: 'good' | 'warn' | 'bad' | 'info' | 'accent'
}>()

const { $frontmatter: fm } = useSlideContext()

const metricValue = computed(() => props.value || fm.value)
const metricUnit = computed(() => props.unit || fm.unit)
const metricLabel = computed(() => props.label || fm.label)
const metricKicker = computed(() => props.kicker || fm.kicker)
const metricTone = computed(() => props.tone || fm.tone || 'accent')

const TONE_CLASSES: Record<string, string> = {
  good: 'text-emerald-500',
  warn: 'text-amber-500',
  bad: 'text-rose-500',
  info: 'text-sky-500',
  accent: 'text-[var(--slidev-theme-primary)]',
}
const isMono = computed(() => fm.mono === true || fm.font === 'mono')
</script>

<template>
  <LayoutBase class="layout-metric">
    <div class="grid grid-cols-1 md:grid-cols-12 gap-10 h-full w-full items-center min-h-0">
      
      <!-- Asymmetric Left Side: Giant Hero Metric (5 cols) -->
      <div class="md:col-span-5 flex flex-col justify-center items-start min-h-0 border-r border-[var(--slidev-theme-border)] pr-8">
        <div v-if="$slots.metric">
          <slot name="metric" />
        </div>
        <div v-else class="flex flex-col">
          <div v-if="metricKicker" class="kicker !mb-2">{{ metricKicker }}</div>
          <div class="flex items-baseline font-extrabold tracking-[-0.04em] leading-none" :class="TONE_CLASSES[metricTone]">
            <span 
              class="text-7xl md:text-8xl lg:text-9xl leading-none"
              :class="isMono ? 'font-mono' : 'font-display'"
              :style="isMono ? {} : { fontFamily: 'var(--slidev-theme-font-header)' }"
            >
              {{ metricValue }}
            </span>
            <span v-if="metricUnit" class="text-3xl md:text-4xl font-bold ml-2 opacity-85 leading-none">{{ metricUnit }}</span>
          </div>
          <div v-if="metricLabel" class="mt-4 text-base md:text-lg font-semibold uppercase tracking-wider text-[var(--slidev-theme-dim)]">
            {{ metricLabel }}
          </div>
        </div>
      </div>

      <!-- Right Side: Narrative Context (7 cols) -->
      <div class="md:col-span-7 flex flex-col justify-center min-h-0 pl-2">
        <slot />
      </div>

    </div>
  </LayoutBase>
</template>
