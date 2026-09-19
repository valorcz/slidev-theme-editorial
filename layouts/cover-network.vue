<script setup lang="ts">
import Cover from './cover.vue'
</script>

<template>
  <Cover class="cover-network">
    <template #tag><slot name="tag">Protocol Analysis // 2026</slot></template>
    <template #default><slot /></template>
    <template #subtitle><slot name="subtitle" /></template>
  </Cover>
</template>