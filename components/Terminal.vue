<script setup lang="ts">
import { computed } from 'vue'

const props = withDefaults(
  defineProps<{
    title?: string
    lines?: Array<string | { cmd?: string; out?: string; comment?: string }>
  }>(),
  {
    title: 'bash',
    lines: () => [],
  }
)

const normalizedLines = computed(() => {
  return props.lines.map(line => {
    if (typeof line === 'string') {
      return { out: line }
    }
    return line
  })
})
</script>

<template>
  <div class="editorial-terminal font-mono rounded-xl border border-[var(--slidev-theme-border)] bg-[var(--slidev-theme-surface)] overflow-hidden shadow-md my-3">
    <!-- Window Bar -->
    <div class="terminal-header flex items-center px-4 py-2.5 bg-[var(--slidev-theme-bg)] border-b border-[var(--slidev-theme-border)] select-none">
      <div class="flex items-center gap-1.5 mr-4">
        <span class="w-3 h-3 rounded-full bg-[#EF4444] opacity-80 inline-block"></span>
        <span class="w-3 h-3 rounded-full bg-[#F59E0B] opacity-80 inline-block"></span>
        <span class="w-3 h-3 rounded-full bg-[#10B981] opacity-80 inline-block"></span>
      </div>
      <div class="flex-1 text-center text-xs font-semibold text-[var(--slidev-theme-dim)] tracking-wider">
        {{ title }}
      </div>
      <div class="w-12"></div>
    </div>

    <!-- Body -->
    <div class="terminal-body p-4 text-xs md:text-sm leading-relaxed overflow-x-auto space-y-1">
      <!-- If lines prop is used -->
      <template v-if="normalizedLines.length > 0">
        <div 
          v-for="(row, idx) in normalizedLines" 
          :key="idx"
          class="terminal-row"
        >
          <!-- Command line -->
          <div v-if="row.cmd" class="flex items-start text-[var(--slidev-theme-color)]">
            <span class="text-[var(--slidev-theme-primary)] mr-2 select-none">$</span>
            <span class="font-semibold">{{ row.cmd }}</span>
          </div>
          <!-- Comment line -->
          <div v-else-if="row.comment" class="text-[var(--slidev-theme-dim)] italic">
            # {{ row.comment }}
          </div>
          <!-- Output line -->
          <div v-else class="text-[var(--slidev-theme-dim)] pl-4">
            {{ row.out }}
          </div>
        </div>
      </template>
      <!-- Otherwise render slot contents (e.g. pre / code / text) -->
      <template v-else>
        <slot />
      </template>
    </div>
  </div>
</template>

<style scoped>
.editorial-terminal :deep(pre) {
  margin: 0 !important;
  padding: 0 !important;
  background: transparent !important;
  border: none !important;
  box-shadow: none !important;
}
</style>
