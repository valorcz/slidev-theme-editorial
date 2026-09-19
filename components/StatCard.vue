<script setup lang="ts">
import { computed } from 'vue'

const props = withDefaults(
  defineProps<{
    value: string | number
    unit?: string
    label?: string
    icon?: string
    tone?: 'good' | 'warn' | 'bad' | 'info' | 'accent' | string
    mono?: boolean
  }>(),
  {
    tone: 'accent',
    mono: false,
  }
)

const TONE_CLASSES: Record<string, string> = {
  good: 'text-emerald-500',
  warn: 'text-amber-500',
  bad: 'text-rose-500',
  info: 'text-sky-500',
  accent: 'text-[var(--slidev-theme-primary)]',
}

const TONE_COLORS: Record<string, string> = {
  good: '#10B981',
  warn: '#F59E0B',
  bad: '#EF4444',
  info: '#3B82F6',
  accent: 'var(--slidev-theme-primary)',
}

const resolvedColor = computed(() => {
  if (!props.tone) return 'var(--slidev-theme-primary)'
  return TONE_COLORS[props.tone] || props.tone
})
</script>

<template>
  <div class="stat-card flex flex-col items-center justify-center p-6 bg-[var(--slidev-theme-surface)] border border-[var(--slidev-theme-border)] rounded-xl transition-all duration-200">
    <div v-if="icon" :class="[icon, 'text-3xl mb-3']" :style="{ color: resolvedColor }"></div>
    <div class="stat-number flex items-baseline justify-center font-extrabold tracking-[-0.03em] leading-none" :style="{ color: resolvedColor }">
      <span 
        class="text-5xl md:text-6xl"
        :class="mono ? 'font-mono' : 'font-display'"
        :style="mono ? {} : { fontFamily: 'var(--slidev-theme-font-header)' }"
      >
        {{ value }}
      </span>
      <span v-if="unit" class="text-2xl font-bold ml-1.5 opacity-85 leading-none">{{ unit }}</span>
    </div>
    <div v-if="label" class="stat-label mt-3 text-sm font-semibold uppercase tracking-wider text-[var(--slidev-theme-dim)] text-center">
      {{ label }}
    </div>
    <div v-if="$slots.default" class="mt-2 text-xs text-[var(--slidev-theme-dim)] text-center">
      <slot />
    </div>
  </div>
</template>

