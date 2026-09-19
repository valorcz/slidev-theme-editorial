<script setup lang="ts">
import { computed } from 'vue'
import { useSlideContext } from '@slidev/client'
import LayoutBase from '../components/LayoutBase.vue'

interface TimelineEvent {
  date?: string
  title: string
  desc?: string
}

const { $frontmatter: fm } = useSlideContext()

// Support Tahta frontmatter fallback: events: [{ date, title, desc }]
const frontmatterEvents = computed<TimelineEvent[]>(() => fm.events || [])
</script>

<template>
  <LayoutBase class="layout-timeline">
    <!-- If frontmatter events exist: render horizontal rail -->
    <div v-if="frontmatterEvents.length > 0" class="timeline-wrapper flex-1 min-h-0 flex flex-col justify-between">
      <div v-if="$slots.default" class="mb-4">
        <slot />
      </div>

      <div class="timeline-horizontal w-full grid grid-flow-col auto-cols-fr gap-8 relative pt-10 my-auto">
        <!-- Horizontal connector line -->
        <div class="absolute left-1.5 right-1.5 top-2 h-0.5 bg-[var(--slidev-theme-border)] opacity-80 pointer-events-none"></div>

        <div 
          v-for="(e, i) in frontmatterEvents" 
          :key="i"
          class="timeline-node relative flex flex-col"
        >
          <!-- Dot on the rail -->
          <div class="w-3.5 h-3.5 rounded-full bg-[var(--slidev-theme-primary)] absolute -top-10 left-0 shadow-[0_0_0_4px_var(--slidev-theme-bg)]"></div>
          
          <!-- Date / Timestamp -->
          <div v-if="e.date" class="text-xs font-mono font-bold uppercase tracking-wider text-[var(--slidev-theme-primary)] mb-2">
            {{ e.date }}
          </div>
          
          <!-- Title -->
          <div class="text-base md:text-lg font-bold text-[var(--slidev-theme-color)] mb-1.5 leading-snug">
            {{ e.title }}
          </div>
          
          <!-- Description -->
          <div v-if="e.desc" class="text-xs md:text-sm text-[var(--slidev-theme-dim)] leading-relaxed max-w-[24ch]">
            {{ e.desc }}
          </div>
        </div>
      </div>
    </div>

    <!-- Otherwise render standard markdown content (title + subtitle + ul) -->
    <div v-else class="timeline-markdown flex-1 min-h-0 flex flex-col">
      <slot />
    </div>
  </LayoutBase>
</template>

<style>
/* Horizontal Timeline Rail for standard Markdown lists */
.layout-timeline .timeline-markdown ul,
.layout-timeline ul {
  display: grid !important;
  grid-auto-flow: column !important;
  grid-auto-columns: minmax(0, 1fr) !important;
  gap: 2.5rem !important;
  position: relative !important;
  padding-top: 0 !important;
  padding-left: 0 !important;
  margin-top: auto !important;
  margin-bottom: auto !important;
  width: 100% !important;
  list-style: none !important;
  border-left: none !important;
}

/* The horizontal connector line */
.layout-timeline .timeline-markdown ul::before,
.layout-timeline ul::before {
  content: "" !important;
  display: block !important;
  position: absolute !important;
  left: 4px !important;
  right: 4px !important;
  top: 6px !important;
  height: 2px !important;
  background-color: var(--slidev-theme-border) !important;
  opacity: 0.8 !important;
  border-radius: 999px !important;
  z-index: 1 !important;
}

/* Individual timeline column */
.layout-timeline .timeline-markdown ul > li,
.layout-timeline ul > li {
  position: relative !important;
  padding-left: 0 !important;
  padding-top: 2rem !important;
  margin-bottom: 0 !important;
  display: flex !important;
  flex-direction: column !important;
  list-style: none !important;
}

/* The dot on the horizontal line */
.layout-timeline .timeline-markdown ul > li::before,
.layout-timeline ul > li::before {
  content: "" !important;
  display: block !important;
  position: absolute !important;
  left: 0 !important;
  top: 0 !important;
  width: 14px !important;
  height: 14px !important;
  border-radius: 50% !important;
  background-color: var(--slidev-theme-primary) !important;
  box-shadow: 0 0 0 4px var(--slidev-theme-bg) !important;
  opacity: 1 !important;
  z-index: 10 !important;
}

/* Heading / Timestamp inside list item */
.layout-timeline ul > li strong,
.layout-timeline ul > li > p > strong:first-child {
  display: block !important;
  font-family: var(--slidev-theme-font-mono) !important;
  font-size: 0.82rem !important;
  font-weight: 700 !important;
  text-transform: uppercase !important;
  letter-spacing: 0.08em !important;
  color: var(--slidev-theme-primary) !important;
  margin-bottom: 0.4rem !important;
  line-height: 1.25 !important;
}

.layout-timeline ul > li p {
  font-size: 0.95rem !important;
  line-height: 1.5 !important;
  color: var(--slidev-theme-dim) !important;
  margin: 0 !important;
  max-width: 28ch !important;
}
</style>

