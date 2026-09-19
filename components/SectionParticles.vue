<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'

const props = withDefaults(
  defineProps<{
    count?: number
    speed?: number
    connectDistance?: number
  }>(),
  {
    count: 36,
    speed: 0.35,
    connectDistance: 130,
  }
)

interface Particle {
  x: number
  y: number
  vx: number
  vy: number
  baseRadius: number
  phase: number
  phaseSpeed: number
}

const canvas = ref<HTMLCanvasElement | null>(null)
let ctx: CanvasRenderingContext2D | null = null
let animationId = 0
let width = 0
let height = 0
let dpr = 1
let resizeObserver: ResizeObserver | null = null
let particles: Particle[] = []

function initParticles() {
  if (width === 0 || height === 0) return
  particles = []
  
  for (let i = 0; i < props.count; i++) {
    const angle = Math.random() * Math.PI * 2
    const speed = (0.2 + Math.random() * 0.45) * props.speed
    particles.push({
      x: Math.random() * width,
      y: Math.random() * height,
      vx: Math.cos(angle) * speed,
      vy: Math.sin(angle) * speed,
      baseRadius: 1.4 + Math.random() * 1.6,
      phase: Math.random() * Math.PI * 2,
      phaseSpeed: 0.015 + Math.random() * 0.02,
    })
  }
}

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

  if (particles.length === 0) {
    initParticles()
  } else {
    // Keep existing particles within bounds upon window resize
    for (const p of particles) {
      if (p.x > width) p.x = Math.random() * width
      if (p.y > height) p.y = Math.random() * height
    }
  }
}

function draw() {
  if (!ctx || !canvas.value || width === 0 || height === 0) return

  // 1. Resolve reactive theme colors
  const style = getComputedStyle(canvas.value)
  const primaryColor = style.getPropertyValue('--slidev-theme-primary').trim() || '#EB5E28'
  const isDark = document.documentElement.classList.contains('dark')

  // Light vs Dark mode styling calibration
  const nodeFill = primaryColor
  const nodeAlphaBase = isDark ? 0.65 : 0.80
  const maxLineAlpha = isDark ? 0.28 : 0.35
  const lineWidth = isDark ? 0.75 : 0.85

  // 2. Clear canvas with transparent buffer
  ctx.clearRect(0, 0, width, height)
  ctx.globalCompositeOperation = 'source-over'

  const maxDist = props.connectDistance
  const maxDistSq = maxDist * maxDist

  // 3. Draw proximity connection lines (Editorial hairline mesh)
  ctx.lineWidth = lineWidth
  ctx.strokeStyle = nodeFill

  for (let i = 0; i < particles.length; i++) {
    const p1 = particles[i]
    for (let j = i + 1; j < particles.length; j++) {
      const p2 = particles[j]
      const dx = p1.x - p2.x
      const dy = p1.y - p2.y
      const distSq = dx * dx + dy * dy

      if (distSq < maxDistSq) {
        const dist = Math.sqrt(distSq)
        const alpha = (1 - dist / maxDist) * maxLineAlpha
        ctx.globalAlpha = alpha
        ctx.beginPath()
        ctx.moveTo(p1.x, p1.y)
        ctx.lineTo(p2.x, p2.y)
        ctx.stroke()
      }
    }
  }

  // 4. Update and draw nodes with gentle breathing pulse
  for (let i = 0; i < particles.length; i++) {
    const p = particles[i]

    // Position updates
    p.x += p.vx
    p.y += p.vy

    // Soft bounds wrap
    if (p.x < -10) p.x = width + 10
    else if (p.x > width + 10) p.x = -10
    if (p.y < -10) p.y = height + 10
    else if (p.y > height + 10) p.y = -10

    // Pulsing alpha and slight radius oscillation
    p.phase += p.phaseSpeed
    const pulse = Math.sin(p.phase)
    const currentRadius = Math.max(1, p.baseRadius + pulse * 0.3)
    const currentAlpha = Math.max(0.2, Math.min(1, nodeAlphaBase + pulse * 0.2))

    ctx.globalAlpha = currentAlpha
    ctx.fillStyle = nodeFill
    ctx.beginPath()
    ctx.arc(p.x, p.y, currentRadius, 0, Math.PI * 2)
    ctx.fill()
  }

  animationId = requestAnimationFrame(draw)
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
  initParticles()
  // Initial frame render (important for static image export)
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
  <canvas 
    ref="canvas" 
    class="section-particles-canvas absolute inset-0 w-full h-full pointer-events-none z-0" 
  />
</template>
