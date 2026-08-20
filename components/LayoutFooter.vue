<script setup lang="ts">
import { useSlideContext } from '@slidev/client'

withDefaults(defineProps<{
  showTitle?: boolean
  showPage?: boolean
}>(), {
  showTitle: true,
  showPage: true
})

const { $slidev, $page } = useSlideContext()
</script>

<template>
  <footer class="absolute bottom-0 left-0 w-full px-14 py-3 flex justify-between items-end pointer-events-none text-[var(--slidev-theme-dim)] z-50">
    
    <div v-if="showTitle" class="text-[0.55rem] font-mono uppercase tracking-widest opacity-60 leading-none">
      {{ $slidev.configs.title || 'Untitled' }}
    </div>
    <div v-else></div>

    <div v-if="showPage" class="text-[0.55rem] font-mono font-bold opacity-40 leading-none">
      {{ $page }} / {{ $slidev.nav.total }}
    </div>

  </footer>
</template>