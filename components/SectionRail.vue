<script setup lang="ts">
import { computed, toValue } from 'vue'
import { useSlideContext } from '@slidev/client'

const { $page, $slidev, $frontmatter: fm } = useSlideContext()

const slides = computed(() => toValue($slidev?.nav?.slides) || [])
const page = computed(() => Number(toValue($page)) || 1)

interface SectionInfo {
  start: number
  count: number
  title: string
}

const isSection = (s: any) => {
  const frontmatter = s?.meta?.slide?.frontmatter || s?.meta?.frontmatter || (s as any)?.frontmatter || {}
  const layout = frontmatter.layout || s?.meta?.layout || (s as any)?.layout
  return layout === 'section'
}

const getSectionTitle = (s: any, fallbackIndex: number) => {
  const frontmatter = s?.meta?.slide?.frontmatter || s?.meta?.frontmatter || (s as any)?.frontmatter || {}
  return frontmatter.title || s?.meta?.slide?.title || s?.title || `Section ${fallbackIndex}`
}

const sections = computed<SectionInfo[]>(() => {
  const list: SectionInfo[] = []
  const arr = slides.value
  if (!arr.length) return list

  for (let i = 0; i < arr.length; i++) {
    const slide = arr[i]
    if (isSection(slide) || list.length === 0) {
      list.push({
        start: i + 1,
        count: 1,
        title: getSectionTitle(slide, list.length + 1),
      })
    } else {
      list[list.length - 1].count++
    }
  }
  return list
})

const curIdx = computed(() => {
  let idx = 0
  sections.value.forEach((s, i) => {
    if (page.value >= s.start) idx = i
  })
  return idx
})

// Can be hidden via frontmatter `rail: false` or `progress: false`
const show = computed(() => {
  if (fm.rail === false || fm.progress === false) return false
  return sections.value.length >= 2
})
</script>

<template>
  <div v-if="show" class="editorial-rail" aria-hidden="true">
    <span
      v-for="(s, i) in sections"
      :key="i"
      class="rail-seg"
      :class="{ done: i < curIdx, current: i === curIdx }"
      :style="{ 
        flexGrow: s.count,
        '--seg-color': `var(--cat-${(i % 5) + 1})`
      }"
      :title="s.title"
    />
  </div>
</template>

