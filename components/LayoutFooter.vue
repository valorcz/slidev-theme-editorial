<script setup lang="ts">
import { useSlideContext } from '@slidev/client'

withDefaults(defineProps<{
  showTitle?: boolean
  showPage?: boolean
}>(), {
  showTitle: true,
  showPage: true
})

const { $slidev } = useSlideContext()
</script>

<template>
  <footer class="absolute bottom-0 left-0 w-full px-16 py-2 flex justify-between items-end pointer-events-none text-[var(--slidev-theme-dim)] z-50">
    
    <div v-if="showTitle" class="text-[0.5rem] font-mono uppercase tracking-widest opacity-50 leading-none">
      {{ $slidev.configs.title || 'Untitled' }}
    </div>
    <div v-else></div>

    <div v-if="showPage" class="text-[0.5rem] font-mono font-bold opacity-30 leading-none">
      {{ $slidev.nav.currentPage }} / {{ $slidev.nav.total }}
    </div>

  </footer>
</template>