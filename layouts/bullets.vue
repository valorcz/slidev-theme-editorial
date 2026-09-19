<script setup lang="ts">
import LayoutBase from '../components/LayoutBase.vue'
</script>

<template>
  <LayoutBase class="bullets">
    <div class="my-auto flex flex-col justify-center">
      <slot />
    </div>
  </LayoutBase>
</template>
