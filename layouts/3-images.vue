<script setup lang="ts">
import { computed } from 'vue'
import { useSlideContext } from '@slidev/client'
import LayoutBase from '../components/LayoutBase.vue'
import { resolveAssetUrl } from '../layoutHelper'

const props = defineProps<{
  imageLeft?: string
  imageTopRight?: string
  imageBottomRight?: string
}>()

const { $frontmatter: fm } = useSlideContext()

const leftSrc = computed(() => {
  const url = props.imageLeft || fm.imageLeft
  return url ? resolveAssetUrl(url) : ''
})

const topRightSrc = computed(() => {
  const url = props.imageTopRight || fm.imageTopRight
  return url ? resolveAssetUrl(url) : ''
})

const bottomRightSrc = computed(() => {
  const url = props.imageBottomRight || fm.imageBottomRight
  return url ? resolveAssetUrl(url) : ''
})
</script>

<template>
  <LayoutBase class="layout-3-images">
    <div v-if="$slots.default" class="mb-4">
      <slot />
    </div>

    <div class="grid grid-cols-2 gap-5 flex-1 min-h-0">
      <!-- Large Hero Image Left -->
      <div 
        class="rounded-xl border border-[var(--slidev-theme-border)] bg-cover bg-center shadow-md grayscale hover:grayscale-0 transition-all duration-700 ease-out"
        :style="{ backgroundImage: `url(${leftSrc})` }"
      />

      <!-- Two Stacked Images Right -->
      <div class="grid grid-rows-2 gap-5 min-h-0">
        <div 
          class="rounded-xl border border-[var(--slidev-theme-border)] bg-cover bg-center shadow-md grayscale hover:grayscale-0 transition-all duration-700 ease-out"
          :style="{ backgroundImage: `url(${topRightSrc})` }"
        />
        <div 
          class="rounded-xl border border-[var(--slidev-theme-border)] bg-cover bg-center shadow-md grayscale hover:grayscale-0 transition-all duration-700 ease-out"
          :style="{ backgroundImage: `url(${bottomRightSrc})` }"
        />
      </div>
    </div>
  </LayoutBase>
</template>
