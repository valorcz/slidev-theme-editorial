<script setup lang="ts">
import Badge from './Badge.vue'

withDefaults(
  defineProps<{
    items?: string[]
    tone?: 'primary' | 'accent' | 'secondary' | 'tertiary' | 'muted' | 'good' | 'warn' | 'bad' | 'info'
    size?: 'sm' | 'md'
  }>(),
  {
    items: () => [],
    tone: 'primary',
    size: 'sm',
  }
)
</script>

<template>
  <div class="tags-container inline-flex flex-wrap items-center gap-2 my-1">
    <template v-if="items.length > 0">
      <Badge 
        v-for="(item, idx) in items" 
        :key="idx" 
        :tone="tone" 
        :size="size"
      >
        {{ item }}
      </Badge>
    </template>
    <slot />
  </div>
</template>
