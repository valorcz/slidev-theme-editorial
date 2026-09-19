<script setup lang="ts">
import { useSlideContext } from '@slidev/client'
import { watchEffect } from 'vue'

const { $slidev } = useSlideContext()

watchEffect(() => {
  // 1. Get the value (fallback to orange)
  let accent = $slidev.configs.accent || '#EB5E28'
  
  // 2. CRITICAL FIX: Sanitize the string. 
  // If the user put quotes in YAML (accent: '#333'), strip them.
  if (typeof accent === 'string') {
    accent = accent.replace(/['"]/g, '').trim()
  }

  // 3. Set the variable on the root <html> element with "important" priority
  // This guarantees it overrides styles/index.css
  if (typeof document !== 'undefined') {
    document.documentElement.style.setProperty('--slidev-theme-primary', accent, 'important')
  }
})
</script>

<template>
  <div />
</template>