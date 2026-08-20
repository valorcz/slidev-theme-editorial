<script setup lang="ts">
import LayoutBase from '../components/LayoutBase.vue'
</script>

<template>
  <LayoutBase class="three-cols-header">
    
    <div class="mb-2">
      <slot />
    </div>

    <div class="grid grid-cols-3 gap-5 flex-1 min-h-0 items-start">
      <div class="h-full min-h-0"><slot name="left" /></div>
      <div class="h-full min-h-0"><slot name="center" /></div>
      <div class="h-full min-h-0"><slot name="right" /></div>
    </div>

  </LayoutBase>
</template>