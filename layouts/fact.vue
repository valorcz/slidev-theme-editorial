<script setup lang="ts">
import { computed, onMounted, ref } from 'vue'
import { useSlideContext } from '@slidev/client'
import LayoutBase from '../components/LayoutBase.vue'

const props = defineProps<{
  value?: string | number
  unit?: string
  label?: string
  kicker?: string
  watermark?: boolean | string
  mono?: boolean
}>()

const { $frontmatter: fm } = useSlideContext()

const containerRef = ref<HTMLElement | null>(null)

const val = computed(() => props.value ?? fm.value)
const unit = computed(() => props.unit ?? fm.unit)
const label = computed(() => props.label ?? fm.label)
const kicker = computed(() => props.kicker ?? fm.kicker)
const showWatermark = computed(() => props.watermark ?? fm.watermark ?? false)
const isMono = computed(() => props.mono || fm.mono || fm.font === 'mono')

// Progressive enhancement: auto-format units in markdown H1 (e.g. 99.9% -> 99.9 + %)
onMounted(() => {
  if (!containerRef.value) return
  const h1 = containerRef.value.querySelector('h1')
  if (h1 && !h1.querySelector('.fact-unit')) {
    const text = h1.textContent?.trim() || ''
    const match = text.match(/^([0-9.,+-]+)\s*([%a-zA-Z/]+)$/)
    if (match) {
      h1.innerHTML = `${match[1]}<span class="fact-unit">${match[2]}</span>`
    }
  }
})
</script>

<template>
  <LayoutBase class="fact" :class="{ 'fact-mono': isMono }" :watermark="showWatermark">
    <div 
      ref="containerRef"
      class="fact-body my-auto flex flex-col justify-center items-center text-center max-w-4xl mx-auto w-full px-6"
    >
      <!-- Optional Kicker -->
      <div v-if="kicker" class="kicker mb-4 select-none">
        {{ kicker }}
      </div>

      <!-- Primary Mode: Standard Markdown Slot (# 99.9% \n Paragraph) -->
      <template v-if="$slots.default">
        <slot />
      </template>

      <!-- Fallback Mode: Frontmatter value/unit/label -->
      <template v-else-if="val !== undefined">
        <h1 class="fact-hero-num">
          <span>{{ val }}</span>
          <span v-if="unit" class="fact-unit">{{ unit }}</span>
        </h1>
        <p v-if="label" class="fact-label" v-html="label" />
      </template>
    </div>
  </LayoutBase>
</template>

<style scoped>
/* Massive Hero Number: DM Sans by default (Editorial Headline Signature) */
.slidev-layout.fact :deep(h1),
.fact-hero-num {
  font-family: var(--slidev-theme-font-header) !important;
  font-size: clamp(6.5rem, 16vw, 11rem) !important;
  font-weight: 800 !important;
  letter-spacing: -0.04em !important;
  line-height: 0.9 !important;
  color: var(--slidev-theme-primary) !important;
  margin-bottom: 1.5rem !important;
  text-align: center !important;
  display: flex !important;
  align-items: baseline !important;
  justify-content: center !important;
  filter: drop-shadow(0 2px 14px color-mix(in srgb, var(--slidev-theme-primary) 18%, transparent));
}

/* Optional Mono override if mono: true */
.slidev-layout.fact.fact-mono :deep(h1),
.slidev-layout.fact.fact-mono .fact-hero-num {
  font-family: var(--slidev-theme-font-mono) !important;
  letter-spacing: -0.05em !important;
}

.slidev-layout.fact :deep(.fact-unit),
.fact-unit {
  font-family: var(--slidev-theme-font-header) !important;
  font-size: 0.5em !important;
  margin-left: 0.12em !important;
  font-weight: 700 !important;
  opacity: 0.85 !important;
  line-height: 1 !important;
  vertical-align: baseline !important;
}

/* Editorial Subtext / Takeaway: Inter */
.slidev-layout.fact :deep(p),
.fact-label {
  font-family: var(--slidev-theme-font-body) !important;
  font-size: clamp(1.25rem, 2.2vw, 1.65rem) !important;
  line-height: 1.5 !important;
  color: var(--slidev-theme-dim) !important;
  max-width: 40ch !important;
  margin: 0 auto !important;
  font-weight: 400 !important;
  letter-spacing: -0.01em !important;
  text-align: center !important;
  opacity: 0.9 !important;
}

/* Optional Kicker: JetBrains Mono uppercase */
.slidev-layout.fact :deep(h4),
.kicker {
  font-family: var(--slidev-theme-font-mono) !important;
  font-size: 0.82rem !important;
  font-weight: 700 !important;
  text-transform: uppercase !important;
  letter-spacing: 0.18em !important;
  color: var(--slidev-theme-primary) !important;
  margin-bottom: 0.75rem !important;
  text-align: center !important;
}
</style>
