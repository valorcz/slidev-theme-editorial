<script setup lang="ts">
import { computed } from 'vue'
import { useSlideContext } from '@slidev/client'
import LayoutBase from '../components/LayoutBase.vue'
import { resolveAssetUrl } from '../layoutHelper'

const props = defineProps<{
  image?: string
  class?: string
}>()

const { $frontmatter: fm } = useSlideContext()
const img = computed(() => props.image || fm.image)
const resolvedImg = computed(() => (img.value ? resolveAssetUrl(img.value) : ''))
</script>

<template>
  <LayoutBase class="intro-image-right" :class="props.class">
    <div class="grid grid-cols-1 md:grid-cols-2 gap-8 h-full w-full items-center min-h-0">
      <div class="my-auto flex flex-col justify-center min-h-0 overflow-y-auto pr-2">
        <slot />
      </div>
      <div class="h-full min-h-0 flex items-center justify-center">
        <div 
          class="w-full h-[85%] rounded-xl bg-cover bg-center border border-[var(--slidev-theme-border)] shadow-md grayscale hover:grayscale-0 transition-all duration-700 ease-out"
          :style="{ backgroundImage: `url(${resolvedImg})` }"
        />
      </div>
    </div>
  </LayoutBase>
</template>
