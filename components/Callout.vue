<script setup lang="ts">
import { computed } from 'vue'

const props = withDefaults(
  defineProps<{
    tone?: 'note' | 'tip' | 'warning' | 'danger' | 'info' | 'good' | 'warn' | 'bad' | 'accent' | 'primary' | 'secondary' | 'tertiary'
    icon?: string
    title?: string
  }>(),
  {
    tone: 'info',
  }
)

const toneConfig = computed(() => {
  switch (props.tone) {
    case 'tip':
    case 'good':
      return {
        border: 'border-emerald-500/60',
        bg: 'bg-emerald-500/10',
        text: 'text-emerald-500',
        icon: props.icon || 'i-carbon-checkmark-outline',
        label: props.title || 'Tip',
      }
    case 'warning':
    case 'warn':
      return {
        border: 'border-amber-500/60',
        bg: 'bg-amber-500/10',
        text: 'text-amber-500',
        icon: props.icon || 'i-carbon-warning-alt',
        label: props.title || 'Warning',
      }
    case 'danger':
    case 'bad':
      return {
        border: 'border-rose-500/60',
        bg: 'bg-rose-500/10',
        text: 'text-rose-500',
        icon: props.icon || 'i-carbon-close-outline',
        label: props.title || 'Danger',
      }
    case 'primary':
    case 'accent':
      return {
        border: 'border-[var(--slidev-theme-primary)]',
        bg: 'bg-[var(--slidev-theme-surface)]',
        text: 'text-[var(--slidev-theme-primary)]',
        icon: props.icon || 'i-carbon-star',
        label: props.title || 'Note',
      }
    case 'secondary':
      return {
        border: 'border-[var(--slidev-theme-secondary)]',
        bg: 'bg-[var(--slidev-theme-surface)]',
        text: 'text-[var(--slidev-theme-secondary)]',
        icon: props.icon || 'i-carbon-tag',
        label: props.title || 'Note',
      }
    case 'tertiary':
      return {
        border: 'border-[var(--slidev-theme-tertiary)]',
        bg: 'bg-[var(--slidev-theme-surface)]',
        text: 'text-[var(--slidev-theme-tertiary)]',
        icon: props.icon || 'i-carbon-tag',
        label: props.title || 'Note',
      }

    case 'note':
    case 'info':
    default:
      return {
        border: 'border-sky-500/60',
        bg: 'bg-sky-500/10',
        text: 'text-sky-500',
        icon: props.icon || 'i-carbon-information',
        label: props.title || 'Info',
      }
  }
})
</script>

<template>
  <div 
    class="callout-box my-3 p-4 rounded-xl border-l-4 border transition-colors flex items-start gap-3"
    :class="[toneConfig.border, toneConfig.bg]"
  >
    <div :class="[toneConfig.icon, toneConfig.text, 'text-xl flex-shrink-0 mt-0.5']"></div>
    <div class="callout-content flex-1 text-sm leading-relaxed text-[var(--slidev-theme-color)]">
      <div v-if="title" class="font-bold text-base mb-1" :class="toneConfig.text">
        {{ title }}
      </div>
      <slot />
    </div>
  </div>
</template>

<style scoped>
.callout-content :deep(p:last-child) {
  margin-bottom: 0;
}
</style>
