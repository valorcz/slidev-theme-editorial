<script setup lang="ts">
import { computed } from 'vue'
import { useSlideContext } from '@slidev/client'
import LayoutBase from '../components/LayoutBase.vue'
import { handleBackground } from '../layoutHelper'

const props = defineProps<{
  image?: string
}>()

const { $frontmatter: fm } = useSlideContext()
const img = computed(() => props.image || fm.image)
const bgStyle = computed(() => handleBackground(img.value, true))
</script>

<template>
  <LayoutBase class="intro-image" :style="bgStyle" :watermark="false">
    <div class="my-auto max-w-3xl flex flex-col justify-center relative z-10">
      <slot />
    </div>
  </LayoutBase>
</template>
