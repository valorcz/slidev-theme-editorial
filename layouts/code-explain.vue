<script setup lang="ts">
import { computed } from 'vue'
import { useSlideContext, useNav } from '@slidev/client'
import LayoutBase from '../components/LayoutBase.vue'

const { $frontmatter: fm, $clicks } = useSlideContext()
const { clicksTotal } = useNav()

// Numbered notes in frontmatter: notes: ['First note', 'Second note']
const notes = computed<string[]>(() => fm.notes || [])
const stepped = computed(() => (clicksTotal?.value ?? 0) > 0)
const cur = computed(() => $clicks?.value ?? 0)
</script>

<template>
  <LayoutBase class="layout-code-explain">
    <div class="code-explain-grid grid grid-cols-1 lg:grid-cols-2 gap-6 flex-1 min-h-0 items-start">
      <!-- Code Column (default slot) -->
      <div class="ce-code flex flex-col min-h-0 overflow-y-auto">
        <slot />
      </div>

      <!-- Notes / Explanation Column -->
      <div class="ce-notes flex flex-col gap-3 min-h-0 overflow-y-auto">
        <!-- Render named slot if present -->
        <template v-if="$slots.notes">
          <slot name="notes" />
        </template>
        <!-- Otherwise fallback to frontmatter notes -->
        <template v-else-if="notes.length > 0">
          <div
            v-for="(n, i) in notes" 
            :key="i"
            class="ce-note-card p-4 rounded-xl border transition-all duration-300 flex items-start gap-3"
            :class="[
              stepped && i === cur
                ? 'border-[var(--slidev-theme-primary)] bg-[var(--slidev-theme-surface)] shadow-md translate-x-1'
                : stepped && i !== cur
                  ? 'opacity-40 border-[var(--slidev-theme-border)] bg-transparent'
                  : 'border-[var(--slidev-theme-border)] bg-[var(--slidev-theme-surface)]'
            ]"
          >
            <span class="font-mono text-xs font-bold px-2 py-0.5 rounded bg-[var(--slidev-theme-bg)] border border-[var(--slidev-theme-border)] text-[var(--slidev-theme-primary)] select-none">
              0{{ i + 1 }}
            </span>
            <div class="text-sm text-[var(--slidev-theme-color)] leading-relaxed" v-html="n" />
          </div>
        </template>
      </div>
    </div>
  </LayoutBase>
</template>
