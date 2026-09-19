<script setup lang="ts">
import { computed } from 'vue'
import { resolveAssetUrl } from '../layoutHelper'

const props = defineProps<{
  src?: string
  alt?: string
  caption?: string
  credit?: string
  fig?: string
  maxHeight?: string
}>()

const resolvedSrc = computed(() => {
  return props.src ? resolveAssetUrl(props.src) : ''
})
</script>

<template>
  <figure class="editorial-figure my-3 flex flex-col items-center">
    <div 
      class="figure-content relative rounded-xl overflow-hidden border border-[var(--slidev-theme-border)] bg-[var(--slidev-theme-surface)] shadow-md w-full flex items-center justify-center"
      :style="maxHeight ? { maxHeight } : null"
    >
      <img 
        v-if="resolvedSrc" 
        :src="resolvedSrc" 
        :alt="alt || caption || ''" 
        class="w-full h-auto object-cover max-h-[380px]"
        :style="maxHeight ? { maxHeight } : null"
      />
      <div v-else class="w-full p-4">
        <slot />
      </div>
    </div>

    <figcaption 
      v-if="caption || credit || fig" 
      class="mt-2.5 flex items-center justify-between w-full px-1 text-xs font-mono text-[var(--slidev-theme-dim)]"
    >
      <div class="flex items-center gap-2">
        <span v-if="fig" class="text-[var(--slidev-theme-primary)] font-bold tracking-wider uppercase">
          {{ fig }}
        </span>
        <span v-if="caption" class="text-[var(--slidev-theme-color)] opacity-85 font-sans text-sm">
          {{ caption }}
        </span>
      </div>

      <span v-if="credit" class="opacity-60 text-[0.7rem] uppercase tracking-wider">
        {{ credit }}
      </span>
    </figcaption>
  </figure>
</template>

<style scoped>
.editorial-figure img {
  display: block;
}
</style>
