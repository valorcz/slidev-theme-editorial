<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'

const canvas = ref<HTMLCanvasElement | null>(null)
let ctx: CanvasRenderingContext2D | null = null
let animationId: number = 0
let width = 0
let height = 0
let time = 0

function resize() {
  if (!canvas.value) return
  width = window.innerWidth
  height = window.innerHeight
  canvas.value.width = width
  canvas.value.height = height
}

function draw() {
  if (!ctx || !canvas.value) return

  // 1. THEME AWARENESS
  const style = getComputedStyle(canvas.value)
  const bgColor = style.getPropertyValue('--slidev-theme-bg') || '#ffffff'
  const primaryColor = style.getPropertyValue('--slidev-theme-primary') || '#06b6d4'

  // 2. CLEARING (The "Trail" Effect)
  // We draw the background color with slight opacity. 
  // Higher opacity (0.2) = shorter, cleaner trails. Lower = longer, messier trails.
  ctx.fillStyle = bgColor 
  
  // Use a hex-to-rgba conversion or specific alpha logic if needed, 
  // but applying globalAlpha with fillRect is the most robust way.
  ctx.globalAlpha = 0.2
  ctx.fillRect(0, 0, width, height)
  
  // 3. DRAWING
  ctx.globalAlpha = 1.0
  ctx.lineWidth = 3
  // "source-over" is standard drawing. No glowing "lighter" mode that distorts colors.
  ctx.globalCompositeOperation = 'source-over' 

  // Wave 1: Primary Theme Color
  drawWave(ctx, primaryColor, 1.0, 0.002, 0.003, 50, 0)
  
  // Wave 2: A slightly dimmer/slower shadow wave for depth
  // We assume the theme color is hex, so we can't easily fade it in canvas without parsing,
  // but we can just use globalAlpha again.
  ctx.globalAlpha = 0.4
  drawWave(ctx, primaryColor, 1.0, 0.003, 0.002, 60, 150)

  time += 1
  animationId = requestAnimationFrame(draw)
}

function drawWave(
  c: CanvasRenderingContext2D, 
  color: string, 
  alpha: number, 
  speed: number, 
  freq: number, 
  amp: number, 
  offset: number
) {
  c.beginPath()
  c.strokeStyle = color
  
  for (let x = 0; x < width; x += 5) {
    // Simple, clean sine wave math
    // y = center + sin(x)
    const y = (height / 2) + 
              Math.sin(x * freq + time * speed) * amp + 
              Math.cos(x * freq * 0.5 + time * speed * 0.5) * 10 // Slight "breathing" wobble

    if (x === 0) c.moveTo(x, y)
    else c.lineTo(x, y)
  }
  c.stroke()
}

onMounted(() => {
  if (!canvas.value) return
  ctx = canvas.value.getContext('2d')
  window.addEventListener('resize', resize)
  resize()
  draw()
})

onUnmounted(() => {
  cancelAnimationFrame(animationId)
  window.removeEventListener('resize', resize)
})
</script>

<template>
  <canvas ref="canvas" class="absolute inset-0 pointer-events-none z-0" />
</template>
