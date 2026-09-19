<script setup lang="ts">
import { computed } from 'vue'
import { useSlideContext } from '@slidev/client'
import LayoutBase from '../components/LayoutBase.vue'
import { resolveAssetUrl } from '../layoutHelper'

const props = defineProps<{
  image?: string
  caption?: string
  figure?: string
}>()

const { $frontmatter: fm } = useSlideContext()

const imgSrc = computed(() => {
  const raw = props.image || fm.image
  return raw ? resolveAssetUrl(raw) : ''
})

const captionText = computed(() => props.caption || fm.caption)
const figLabel = computed(() => props.figure || fm.figure || 'FIG 1.0')
</script>

<template>
  <LayoutBase class="layout-image-right">
    <div class="grid grid-cols-1 md:grid-cols-2 gap-8 h-full w-full items-center min-h-0">
      
      <!-- Text / Narrative Column -->
      <div class="flex flex-col justify-center min-h-0 overflow-y-auto pr-2">
        <slot />
      </div>

      <!-- Editorial Image Column -->
      <div class="h-full min-h-0 relative flex flex-col justify-center">
        <div 
          class="relative w-full h-[85%] rounded-xl overflow-hidden border border-[var(--slidev-theme-border)] shadow-md group"
        >
          <div 
            class="w-full h-full bg-cover bg-center grayscale contrast-105 group-hover:grayscale-0 transition-all duration-700 ease-out"
            :style="{ backgroundImage: `url(${imgSrc})` }"
          />

          <!-- Editorial Caption Badge -->
          <div 
            v-if="captionText" 
            class="absolute bottom-0 right-0 backdrop-blur-sm px-3.5 py-1.5 border-t border-l border-[var(--slidev-theme-border)] font-mono text-xs text-[var(--slidev-theme-dim)] rounded-tl-lg"
            style="background-color: color-mix(in srgb, var(--slidev-theme-bg) 90%, transparent);"
          >
            <span class="text-[var(--slidev-theme-primary)] font-bold mr-1.5">{{ figLabel }}</span>
            <span>{{ captionText }}</span>
          </div>
        </div>
      </div>

    </div>
  </LayoutBase>
</template>