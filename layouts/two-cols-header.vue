<script setup lang="ts">
import LayoutBase from '../components/LayoutBase.vue'
</script>

<template>
  <LayoutBase class="two-cols-header">
    
    <div class="mb-2">
      <slot />
    </div>

    <div class="grid grid-cols-2 gap-6 flex-1 min-h-0">
      <div class="flex flex-col min-h-0">
        <slot name="left" />
      </div>
      <div class="flex flex-col min-h-0 relative">
        <slot name="right" />
      </div>
    </div>

  </LayoutBase>
</template>