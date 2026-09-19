<script setup lang="ts">
import { computed } from 'vue'
import { useSlideContext } from '@slidev/client'
import LayoutBase from '../components/LayoutBase.vue'

interface CompareRow {
  metric: string
  before: string
  after: string
  delta?: string
}

const { $frontmatter: fm } = useSlideContext()

const rows = computed<CompareRow[]>(() => fm.rows || [])
const cols = computed<string[]>(() => fm.columns || ['Metric', 'Before', 'After', 'Δ Delta'])
</script>

<template>
  <LayoutBase class="layout-compare">
    <!-- Header title/subtitle from default slot -->
    <div class="mb-4">
      <slot />
    </div>

    <!-- Frontmatter rows fallback -->
    <div v-if="rows.length > 0" class="compare-table-wrapper flex-1 min-h-0 overflow-y-auto">
      <table class="editorial-compare-table w-full text-left border-collapse">
        <thead>
          <tr class="border-b-2 border-[var(--slidev-theme-border)]">
            <th 
              v-for="(c, i) in cols" 
              :key="i"
              class="py-3 px-4 font-mono text-xs uppercase tracking-wider text-[var(--slidev-theme-dim)] font-bold"
            >
              {{ c }}
            </th>
          </tr>
        </thead>
        <tbody class="divide-y divide-[var(--slidev-theme-border)]/50">
          <tr 
            v-for="(r, i) in rows" 
            :key="i"
            class="hover:bg-[var(--slidev-theme-surface)] transition-colors"
          >
            <td class="py-3.5 px-4 font-semibold text-[var(--slidev-theme-color)]">
              {{ r.metric }}
            </td>
            <td class="py-3.5 px-4 text-[var(--slidev-theme-dim)] font-mono">
              {{ r.before }}
            </td>
            <td class="py-3.5 px-4 font-bold text-[var(--slidev-theme-primary)] font-mono">
              {{ r.after }}
            </td>
            <td v-if="r.delta !== undefined" class="py-3.5 px-4 font-mono text-xs font-semibold text-emerald-500">
              {{ r.delta }}
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    <!-- Slotted markdown table -->
    <div v-else class="compare-markdown flex-1 min-h-0 overflow-y-auto">
      <slot name="content" />
    </div>
  </LayoutBase>
</template>

<style>
/* Auto-styling standard Markdown tables inside layout-compare */
.layout-compare table {
  width: 100% !important;
  border-collapse: collapse !important;
  margin-top: 1rem !important;
  font-size: 0.95rem !important;
}

.layout-compare thead th {
  font-family: var(--slidev-theme-font-mono) !important;
  font-size: 0.8rem !important;
  text-transform: uppercase !important;
  letter-spacing: 0.12em !important;
  color: var(--slidev-theme-dim) !important;
  border-bottom: 2px solid var(--slidev-theme-border) !important;
  padding: 0.75rem 1rem !important;
  text-align: left !important;
}

.layout-compare tbody td {
  padding: 0.85rem 1rem !important;
  border-bottom: 1px solid color-mix(in srgb, var(--slidev-theme-border) 60%, transparent) !important;
  color: var(--slidev-theme-color) !important;
}

.layout-compare tbody tr:hover td {
  background-color: var(--slidev-theme-surface) !important;
}

.layout-compare tbody td:first-child {
  font-weight: 600 !important;
}
</style>
