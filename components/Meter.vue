<script setup lang="ts">
import { computed } from 'vue'

const props = withDefaults(
  defineProps<{
    value: number
    max?: number
    label?: string
    unit?: string
    tone?: 'good' | 'warn' | 'bad' | 'info' | 'accent' | 'primary' | 'secondary' | 'tertiary'
  }>(),
  {
    max: 100,
    unit: '%',
    tone: 'accent',
  }
)

const percentage = computed(() => {
  const pct = (props.value / props.max) * 100
  return Math.min(Math.max(pct, 0), 100)
})

const barColor = computed(() => {
  switch (props.tone) {
    case 'good': return '#10B981'
    case 'warn': return '#F59E0B'
    case 'bad': return '#EF4444'
    case 'info': return '#3B82F6'
    case 'secondary': return 'var(--slidev-theme-secondary)'
    case 'tertiary': return 'var(--slidev-theme-tertiary)'
    case 'primary':
    case 'accent':
    default:
      return 'var(--slidev-theme-primary)'
  }
})

</script>

<template>
  <div class="editorial-meter my-2">
    <div v-if="label || value !== undefined" class="flex justify-between items-baseline mb-1.5 text-xs font-mono">
      <span v-if="label" class="text-[var(--slidev-theme-dim)] uppercase tracking-wider font-semibold">{{ label }}</span>
      <span class="font-bold text-[var(--slidev-theme-color)]">{{ value }}{{ unit }}</span>
    </div>
    <div class="w-full h-2.5 bg-[var(--slidev-theme-border)]/40 rounded-full overflow-hidden p-0.5 border border-[var(--slidev-theme-border)]">
      <div 
        class="h-full rounded-full transition-all duration-500 ease-out" 
        :style="{ width: `${percentage}%`, backgroundColor: barColor }"
      ></div>
    </div>
  </div>
</template>
