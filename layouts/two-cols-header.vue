<script setup lang="ts">
import LayoutBase from '../components/LayoutBase.vue'
</script>

<template>
  <LayoutBase class="two-cols-header">
    
    <div class="mb-6">
      <slot />
    </div>

    <div class="grid grid-cols-2 gap-8 h-full">
      <div class="flex flex-col">
        <slot name="left" />
      </div>
      <div class="flex flex-col relative">
        <slot name="right" />
      </div>
    </div>

  </LayoutBase>
</template>