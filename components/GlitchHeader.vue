<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'

const props = defineProps<{ text: string }>()
const display = ref('')
const chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ1234567890@#&'
let interval: ReturnType<typeof setInterval> | null = null

onMounted(() => {
  let iteration = 0
  interval = setInterval(() => {
    display.value = props.text
      .split('')
      .map((letter, index) => {
        if (index < iteration) return props.text[index]
        return chars[Math.floor(Math.random() * chars.length)]
      })
      .join('')

    if (iteration >= props.text.length) {
      if (interval) clearInterval(interval)
      interval = null
    }
    iteration += 1 / 2
  }, 30)
})

onUnmounted(() => {
  if (interval) clearInterval(interval)
})
</script>

<template>
  <h2 class="text-5xl font-bold uppercase tracking-tighter text-[var(--slidev-theme-primary)]">
    {{ display }}
  </h2>
</template>