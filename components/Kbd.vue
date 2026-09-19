<script setup lang="ts">
withDefaults(
  defineProps<{
    tone?: 'default' | 'accent'
  }>(),
  {
    tone: 'default',
  }
)
</script>

<template>
  <kbd 
    class="kbd inline-flex items-center justify-center font-mono font-semibold text-[0.78em] leading-none px-2 py-1 rounded-md border select-none transition-colors align-baseline mx-0.5"
    :class="[
      tone === 'accent' ? 'kbd--accent' : 'kbd--default'
    ]"
  >
    <slot />
  </kbd>
</template>

<style scoped>
.kbd {
  box-shadow: 0 1px 0 rgba(0, 0, 0, 0.1);
}

.kbd--default {
  border-color: var(--slidev-theme-border);
  border-bottom-width: 2px;
  border-bottom-color: var(--slidev-theme-border);
  color: var(--slidev-theme-color);
  background-color: var(--slidev-theme-surface);
}

.kbd--accent {
  border-color: color-mix(in srgb, var(--slidev-theme-primary) 40%, transparent);
  border-bottom-width: 2px;
  border-bottom-color: var(--slidev-theme-primary);
  background-color: color-mix(in srgb, var(--slidev-theme-primary) 14%, transparent);
  color: var(--slidev-theme-primary);
}

:global(html:not(.dark)) .kbd--accent {
  background-color: color-mix(in srgb, var(--slidev-theme-primary) 12%, transparent);
  color: color-mix(in oklab, var(--slidev-theme-primary) 65%, black);
}
</style>

