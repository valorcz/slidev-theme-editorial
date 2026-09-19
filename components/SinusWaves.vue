<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'

const canvas = ref<HTMLCanvasElement | null>(null)
let ctx: CanvasRenderingContext2D | null = null
let animationId: number = 0
let width = 0
let height = 0
let dpr = 1
let time = 0
let resizeObserver: ResizeObserver | null = null

function resize() {
  if (!canvas.value) return
  const parent = canvas.value.parentElement || canvas.value
  const rect = parent.getBoundingClientRect()
  
  width = rect.width || 980
  height = rect.height || 552
  dpr = window.devicePixelRatio || 1

  canvas.value.width = Math.round(width * dpr)
  canvas.value.height = Math.round(height * dpr)
  
  if (ctx) {
    ctx.setTransform(1, 0, 0, 1, 0, 0)
    ctx.scale(dpr, dpr)
  }
}

function draw() {
  if (!ctx || !canvas.value || width === 0 || height === 0) return

  // 1. THEME AWARENESS
  const style = getComputedStyle(canvas.value)
  const primaryColor = style.getPropertyValue('--slidev-theme-primary').trim() || '#EB5E28'

  // 2. TRANSPARENT CLEARING (Eliminates opaque bounding box artifact)
  ctx.clearRect(0, 0, width, height)

  // 3. DRAWING WAVES (Positioned in lower area to frame content)
  ctx.globalCompositeOperation = 'source-over'

  // Wave 1: Primary Accent Wave
  ctx.globalAlpha = 0.65
  ctx.lineWidth = 2.5
  drawWave(ctx, primaryColor, 0.002, 0.003, 35, 0, height * 0.65)

  // Wave 2: Slower Secondary Accent Depth Wave
  ctx.globalAlpha = 0.25
  ctx.lineWidth = 1.5
  drawWave(ctx, primaryColor, 0.003, 0.002, 45, 120, height * 0.72)

  time += 1
  animationId = requestAnimationFrame(draw)
}

function drawWave(
  c: CanvasRenderingContext2D, 
  color: string, 
  speed: number, 
  freq: number, 
  amp: number, 
  offset: number,
  baseY: number
) {
  c.beginPath()
  c.strokeStyle = color
  
  for (let x = -10; x <= width + 10; x += 6) {
    const y = baseY + 
              Math.sin((x + offset) * freq + time * speed) * amp + 
              Math.cos((x + offset) * freq * 0.5 + time * speed * 0.4) * 8

    if (x === -10) c.moveTo(x, y)
    else c.lineTo(x, y)
  }
  c.stroke()
}

onMounted(() => {
  if (!canvas.value) return
  ctx = canvas.value.getContext('2d')
  
  if (typeof ResizeObserver !== 'undefined' && canvas.value.parentElement) {
    resizeObserver = new ResizeObserver(() => {
      resize()
    })
    resizeObserver.observe(canvas.value.parentElement)
  } else {
    window.addEventListener('resize', resize)
  }

  resize()
  draw()
})

onUnmounted(() => {
  if (animationId) cancelAnimationFrame(animationId)
  if (resizeObserver) {
    resizeObserver.disconnect()
    resizeObserver = null
  }
  window.removeEventListener('resize', resize)
})
</script>

<template>
  <canvas ref="canvas" class="absolute inset-0 w-full h-full pointer-events-none z-0" />
</template>
