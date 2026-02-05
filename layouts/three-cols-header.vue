<script setup lang="ts">
import LayoutBase from '../components/LayoutBase.vue'
</script>

<template>
  <LayoutBase class="three-cols-header">
    
    <div class="mb-8">
      <slot />
    </div>

    <div class="grid grid-cols-3 gap-6 h-full items-start">
      <div class="h-full"><slot name="left" /></div>
      <div class="h-full"><slot name="center" /></div>
      <div class="h-full"><slot name="right" /></div>
    </div>

  </LayoutBase>
</template>