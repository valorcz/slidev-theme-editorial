<script setup lang="ts">
import LayoutBase from '../components/LayoutBase.vue'
</script>

<template>
  <LayoutBase class="intro">
    <div class="my-auto flex flex-col justify-center max-w-3xl">
      <slot />
    </div>
  </LayoutBase>
</template>
