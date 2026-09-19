<script setup lang="ts">
import { computed } from 'vue'

export interface FileTreeNode {
  name: string
  dir?: boolean
  children?: FileTreeNode[]
  desc?: string
  highlight?: boolean
}

const props = withDefaults(
  defineProps<{
    items?: FileTreeNode[]
    title?: string
  }>(),
  {
    items: () => [],
  }
)

interface FlatRow {
  guide: string
  name: string
  dir: boolean
  desc?: string
  highlight?: boolean
}

function flatten(nodes: FileTreeNode[], prefix = '', out: FlatRow[] = []): FlatRow[] {
  nodes.forEach((node, idx) => {
    const isLast = idx === nodes.length - 1
    const isDir = node.dir || (Array.isArray(node.children) && node.children.length > 0)
    
    out.push({
      guide: prefix + (isLast ? '└─ ' : '├─ '),
      name: node.name,
      dir: isDir,
      desc: node.desc,
      highlight: node.highlight,
    })

    if (Array.isArray(node.children) && node.children.length > 0) {
      flatten(node.children, prefix + (isLast ? '   ' : '│  '), out)
    }
  })
  return out
}

const flatRows = computed(() => flatten(props.items))
</script>

<template>
  <div class="filetree-container font-mono text-xs md:text-sm rounded-xl border border-[var(--slidev-theme-border)] bg-[var(--slidev-theme-surface)] overflow-hidden shadow-md my-3">
    <!-- Header bar if title provided -->
    <div v-if="title" class="px-4 py-2 bg-[var(--slidev-theme-bg)] border-b border-[var(--slidev-theme-border)] flex items-center gap-2 select-none text-xs font-semibold text-[var(--slidev-theme-dim)]">
      <span class="i-carbon-folder text-[var(--slidev-theme-primary)] text-sm"></span>
      <span>{{ title }}</span>
    </div>

    <!-- Tree Body -->
    <div class="p-4 leading-relaxed overflow-x-auto space-y-1">
      <template v-if="flatRows.length > 0">
        <div 
          v-for="(row, idx) in flatRows" 
          :key="idx"
          class="filetree-row flex items-baseline select-none"
          :class="{ 'text-[var(--slidev-theme-primary)] font-bold': row.highlight }"
        >
          <span class="guide text-[var(--slidev-theme-dim)] opacity-60 mr-1.5 whitespace-pre">
            {{ row.guide }}
          </span>
          <span class="node-icon mr-1.5 opacity-70 text-xs">
            <span v-if="row.dir" class="i-carbon-folder text-[var(--slidev-theme-primary)]"></span>
            <span v-else class="i-carbon-document text-[var(--slidev-theme-dim)]"></span>
          </span>
          <span 
            class="node-name"
            :class="[
              row.dir ? 'font-semibold text-[var(--slidev-theme-color)]' : 'text-[var(--slidev-theme-color)] opacity-90'
            ]"
          >
            {{ row.name }}
          </span>
          <span v-if="row.desc" class="ml-3 text-xs text-[var(--slidev-theme-dim)] opacity-70 font-sans italic">
            // {{ row.desc }}
          </span>
        </div>
      </template>

      <!-- Slotted fallback (for ASCII trees) -->
      <template v-else>
        <div class="filetree-slotted whitespace-pre font-mono">
          <slot />
        </div>
      </template>
    </div>
  </div>
</template>

<style scoped>
.filetree-slotted :deep(pre) {
  margin: 0 !important;
  padding: 0 !important;
  background: transparent !important;
  border: none !important;
}
</style>
