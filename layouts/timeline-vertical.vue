<script setup lang="ts">
import { computed } from 'vue'
import { useSlideContext } from '@slidev/client'
import LayoutBase from '../components/LayoutBase.vue'

interface TimelineEvent {
  date?: string
  title: string
  desc?: string
  tag?: string
  badge?: string
}

const { $frontmatter: fm } = useSlideContext()

// Support frontmatter events: [{ date, title, desc, tag }]
const frontmatterEvents = computed<TimelineEvent[]>(() => fm.events || [])
const isTwoCols = computed(() => fm.columns === 2 || fm.cols === 2)
</script>

<template>
  <LayoutBase class="layout-timeline-vertical">
    <div class="flex-1 min-h-0 flex flex-col justify-between">
      
      <!-- Primary Title Block -->
      <div v-if="$slots.default && frontmatterEvents.length > 0" class="mb-2">
        <slot />
      </div>

      <!-- Mode 1: Frontmatter Events -->
      <div 
        v-if="frontmatterEvents.length > 0" 
        class="timeline-v-container flex-1 min-h-0 flex flex-col justify-center my-auto"
      >
        <div 
          class="timeline-v-rail relative pl-8 border-l-2 border-[var(--slidev-theme-border)] space-y-6 ml-4 my-auto"
          :class="isTwoCols ? 'grid grid-cols-2 gap-x-10 gap-y-6 space-y-0' : ''"
        >
          <div 
            v-for="(e, i) in frontmatterEvents" 
            :key="i"
            class="timeline-v-node relative flex flex-col"
          >
            <!-- Dot on the vertical spine -->
            <div class="w-3.5 h-3.5 rounded-full bg-[var(--slidev-theme-primary)] absolute -left-[39px] top-1 shadow-[0_0_0_4px_var(--slidev-theme-bg)] z-10"></div>
            
            <div class="flex items-baseline gap-2.5 flex-wrap">
              <!-- Timestamp / Date -->
              <span v-if="e.date" class="font-mono text-xs font-bold uppercase tracking-wider text-[var(--slidev-theme-primary)]">
                {{ e.date }}
              </span>
              <!-- Event Title -->
              <span class="text-base font-bold text-[var(--slidev-theme-color)]">
                {{ e.title }}
              </span>
              <!-- Optional Tag -->
              <span v-if="e.tag || e.badge" class="px-2 py-0.5 text-[0.65rem] font-mono uppercase tracking-wider rounded bg-[var(--slidev-theme-surface)] border border-[var(--slidev-theme-border)] text-[var(--slidev-theme-dim)]">
                {{ e.tag || e.badge }}
              </span>
            </div>
            
            <!-- Description -->
            <div v-if="e.desc" class="text-sm text-[var(--slidev-theme-dim)] mt-1 leading-relaxed max-w-3xl">
              {{ e.desc }}
            </div>
          </div>
        </div>
      </div>

      <!-- Mode 2: Slotted Markdown Content -->
      <div v-else class="timeline-v-markdown flex-1 min-h-0 flex flex-col justify-between">
        <slot />
      </div>

    </div>
  </LayoutBase>
</template>

<style>
/* Vertical Timeline Spine for standard Markdown lists */
.layout-timeline-vertical .timeline-v-markdown ul {
  position: relative !important;
  margin-top: auto !important;
  margin-bottom: auto !important;
  padding-left: 2rem !important;
  border-left: 2px solid var(--slidev-theme-border) !important;
  list-style: none !important;
  display: flex !important;
  flex-direction: column !important;
  gap: 1.5rem !important;
  margin-left: 1rem !important;
}

/* Individual event node */
.layout-timeline-vertical .timeline-v-markdown ul > li {
  position: relative !important;
  padding-left: 0 !important;
  margin-bottom: 0 !important;
  list-style: none !important;
  display: flex !important;
  flex-direction: column !important;
}

/* The node dot on the vertical spine */
.layout-timeline-vertical .timeline-v-markdown ul > li::before {
  content: "" !important;
  display: block !important;
  position: absolute !important;
  left: calc(-2rem - 8px) !important;
  top: 0.25rem !important;
  width: 14px !important;
  height: 14px !important;
  border-radius: 50% !important;
  background-color: var(--slidev-theme-primary) !important;
  box-shadow: 0 0 0 4px var(--slidev-theme-bg) !important;
  z-index: 10 !important;
}

/* Date / Title heading inside list item */
.layout-timeline-vertical .timeline-v-markdown ul > li strong,
.layout-timeline-vertical .timeline-v-markdown ul > li > p > strong:first-child {
  display: inline-block !important;
  font-family: var(--slidev-theme-font-mono) !important;
  font-size: 0.95rem !important;
  font-weight: 700 !important;
  letter-spacing: 0.03em !important;
  color: var(--slidev-theme-color) !important;
  margin-bottom: 0.2rem !important;
  line-height: 1.35 !important;
}

/* Paragraph text */
.layout-timeline-vertical .timeline-v-markdown ul > li p {
  font-size: 0.95rem !important;
  line-height: 1.5 !important;
  color: var(--slidev-theme-dim) !important;
  margin: 0 !important;
  max-width: 75ch !important;
}
</style>
