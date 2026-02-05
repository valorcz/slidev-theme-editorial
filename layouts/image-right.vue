<template>
  <div class="slidev-layout w-full h-full grid grid-cols-2">
    
    <div class="p-12 flex flex-col justify-center relative border-r border-[var(--slidev-theme-border)]">
      <slot />
    </div>

    <div 
      class="h-full w-full bg-cover bg-center grayscale hover:grayscale-0 transition-all duration-700 ease-out"
      :style="{ backgroundImage: `url(${image})` }"
    >
      <div v-if="caption" class="absolute bottom-0 right-0 bg-[var(--slidev-theme-bg)] p-4 border-t border-l border-[var(--slidev-theme-border)] font-mono text-xs">
        FIG 1.0 — {{ caption }}
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
defineProps<{
  image: string
  caption?: string
}>()
</script>