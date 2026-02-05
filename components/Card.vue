<script setup lang="ts">
defineProps<{
  title?: string
  icon?: string
  class?: string // Allow overriding styles (e.g. background color)
}>()
</script>

<template>
  <div class="card-box" :class="class">
    <div v-if="icon" :class="[icon, 'text-4xl mb-3 text-[var(--slidev-theme-primary)]']"></div>
    
    <h3 v-if="title" class="!mb-2">{{ title }}</h3>
    
    <div class="text-[var(--slidev-theme-dim)] leading-relaxed">
      <slot />
    </div>
  </div>
</template>