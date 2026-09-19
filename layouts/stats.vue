<script setup lang="ts">
import { computed } from 'vue'
import { useSlideContext } from '@slidev/client'
import LayoutBase from '../components/LayoutBase.vue'
import Stat from '../components/Stat.vue'

const { $frontmatter: fm } = useSlideContext()

// Support Tahta frontmatter: stats: [{ value, unit, label, tone, icon }]
const frontmatterStats = computed(() => fm.stats || [])
const cols = computed(() => fm.columns || Math.min(frontmatterStats.value.length || 3, 4))
const statSize = computed(() => (cols.value >= 4 ? 'lg' : 'xl'))
</script>

<template>
  <LayoutBase class="layout-stats">
    <div class="layout-stats-inner flex-1 flex flex-col justify-between min-h-0">
      
      <!-- Fallback Title Block if passed in default slot along with frontmatter stats -->
      <div v-if="$slots.default && frontmatterStats.length > 0" class="mb-2">
        <slot />
      </div>

      <!-- Mode 1: Frontmatter stats (Tahta syntax) -->
      <div 
        v-if="frontmatterStats.length > 0" 
        class="stats-body flex-1 flex items-center justify-center min-h-0"
      >
        <div 
          class="stat-grid grid w-full items-center gap-10 md:gap-14"
          :style="{ gridTemplateColumns: `repeat(${cols}, minmax(0, 1fr))` }"
        >
          <Stat
            v-for="(s, i) in frontmatterStats" 
            :key="i"
            :value="s.value"
            :unit="s.unit"
            :label="s.label"
            :tone="s.tone"
            :icon="s.icon"
            :size="statSize"
          />
        </div>
      </div>

      <!-- Mode 2: Slotted Markdown Content -->
      <div v-else class="stats-slot-wrapper flex-1 flex flex-col justify-between min-h-0">
        <slot />
      </div>

    </div>
  </LayoutBase>
</template>

<style>
/* CSS for stat-grid when used in markdown slot */
.layout-stats .stat-grid {
  display: grid;
  gap: 2.5rem;
  width: 100%;
  align-items: center;
  justify-content: center;
  margin: auto 0;
}

/* Auto-styling standard markdown lists in layout: stats into a horizontal stats row */
.layout-stats .stats-slot-wrapper > ul {
  display: grid;
  grid-auto-flow: column;
  grid-auto-columns: minmax(0, 1fr);
  gap: 2.5rem;
  width: 100%;
  margin: auto 0;
  padding: 0;
  text-align: center;
}

.layout-stats .stats-slot-wrapper > ul > li {
  list-style: none;
  padding-left: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 0;
}

.layout-stats .stats-slot-wrapper > ul > li::before {
  display: none;
}

.layout-stats .stats-slot-wrapper > ul > li strong,
.layout-stats .stats-slot-wrapper > ul > li b {
  font-family: var(--slidev-theme-font-header);
  font-weight: 800;
  font-size: 5.5rem;
  letter-spacing: -0.04em;
  line-height: 0.9;
  color: var(--slidev-theme-primary);
  display: block;
  margin-bottom: 0.5rem;
}

.layout-stats .stats-slot-wrapper > ul > li p {
  margin: 0;
  max-width: none;
  font-size: 1.1rem;
  color: var(--slidev-theme-dim);
}
</style>

