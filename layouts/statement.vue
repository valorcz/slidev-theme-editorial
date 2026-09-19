<script setup lang="ts">
import LayoutBase from '../components/LayoutBase.vue'
</script>

<template>
  <LayoutBase class="statement">
    <div class="my-auto flex flex-col justify-center text-center max-w-3xl mx-auto">
      <slot />
    </div>
  </LayoutBase>
</template>
