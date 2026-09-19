<script setup lang="ts">
import { computed } from 'vue'

const props = withDefaults(
  defineProps<{
    tone?: 'primary' | 'accent' | 'secondary' | 'tertiary' | 'muted' | 'good' | 'warn' | 'bad' | 'info'
    size?: 'sm' | 'md'
  }>(),
  {
    tone: 'primary',
    size: 'sm',
  }
)

const resolvedTone = computed(() => {
  if (props.tone === 'accent') return 'primary'
  return props.tone || 'primary'
})
</script>

<template>
  <span 
    class="badge editorial-badge"
    :class="[
      `editorial-badge--${resolvedTone}`,
      `editorial-badge--${size}`
    ]"
  >
    <slot />
  </span>
</template>

