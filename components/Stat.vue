<script setup lang="ts">
import { computed } from 'vue'

const props = withDefaults(
  defineProps<{
    value: string | number
    unit?: string
    label?: string
    icon?: string
    tone?: 'good' | 'warn' | 'bad' | 'info' | 'accent' | string
    size?: 'sm' | 'md' | 'lg' | 'xl'
    mono?: boolean
  }>(),
  {
    tone: 'accent',
    size: 'xl',
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
  primary: 'var(--slidev-theme-primary)',
  secondary: 'var(--slidev-theme-secondary)',
  tertiary: 'var(--slidev-theme-tertiary)',
}


const resolvedColor = computed(() => {
  if (!props.tone) return 'var(--slidev-theme-primary)'
  return TONE_COLORS[props.tone] || props.tone
})
</script>

<template>
  <div class="stat flex flex-col items-center text-center">
    <!-- Icon -->
    <div 
      v-if="icon" 
      :class="[icon, 'stat-icon text-3xl mb-3']"
      :style="{ color: resolvedColor }"
    ></div>

    <!-- Number + Unit -->
    <div 
      class="stat-num flex items-baseline justify-center font-extrabold tracking-[-0.04em] leading-[0.9]"
      :style="{ color: resolvedColor }"
    >
      <span 
        :class="[
          mono ? 'font-mono' : 'font-display',
          size === 'sm' ? 'text-3xl' : size === 'md' ? 'text-4xl md:text-5xl' : size === 'lg' ? 'text-5xl md:text-6xl' : 'text-6xl md:text-7xl lg:text-8xl'
        ]"
        :style="mono ? {} : { fontFamily: 'var(--slidev-theme-font-header)' }"
      >
        {{ value }}
      </span>
      <span 
        v-if="unit" 
        class="stat-unit font-bold ml-1.5 opacity-85 leading-none"
        :class="size === 'sm' ? 'text-sm' : size === 'md' ? 'text-xl' : size === 'lg' ? 'text-2xl' : 'text-3xl md:text-4xl'"
      >
        {{ unit }}
      </span>
    </div>

    <!-- Label -->
    <div v-if="label" class="stat-label mt-3 text-base md:text-lg font-medium text-[var(--slidev-theme-dim)]">
      {{ label }}
    </div>

    <!-- Optional extra slot content -->
    <div v-if="$slots.default" class="mt-2 text-sm text-[var(--slidev-theme-dim)]">
      <slot />
    </div>
  </div>
</template>

