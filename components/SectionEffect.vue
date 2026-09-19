<script setup lang="ts">
import { computed } from 'vue'
import SinusWaves from './SinusWaves.vue'
import TechGrid from './TechGrid.vue'
import AmbientGlow from './AmbientGlow.vue'
import HeroCube from './HeroCube.vue'
import SectionParticles from './SectionParticles.vue'

const props = withDefaults(
  defineProps<{
    effect?: string
    seed?: number
  }>(),
  {
    effect: 'waves',
    seed: 0,
  }
)

const AVAILABLE_EFFECTS = ['waves', 'grid', 'glow', 'cube', 'particles'] as const

const activeEffect = computed(() => {
  const eff = (props.effect || 'waves').toLowerCase().trim()
  
  if (eff === 'none' || eff === 'false') return 'none'

  if (eff === 'random') {
    // Deterministic selection based on slide/section seed to prevent flickering across navigation
    const idx = Math.abs(props.seed || 0) % AVAILABLE_EFFECTS.length
    return AVAILABLE_EFFECTS[idx]
  }

  if (AVAILABLE_EFFECTS.includes(eff as any)) {
    return eff
  }

  return 'waves'
})
</script>

<template>
  <div class="section-effect-container absolute inset-0 pointer-events-none z-0 overflow-hidden">
    <!-- Waves Canvas Effect -->
    <SinusWaves v-if="activeEffect === 'waves'" />

    <!-- Particles Constellation Mesh Effect -->
    <SectionParticles v-else-if="activeEffect === 'particles'" />

    <!-- Technical Dot-Matrix Grid Effect -->
    <TechGrid v-else-if="activeEffect === 'grid'" />

    <!-- Ambient Glowing Orbs Effect -->
    <AmbientGlow v-else-if="activeEffect === 'glow'" />

    <!-- 3D Geometric Cube Background Effect -->
    <div 
      v-else-if="activeEffect === 'cube'" 
      class="absolute -right-12 top-1/2 -translate-y-1/2 pointer-events-none opacity-25 scale-125 z-0"
    >
      <HeroCube size="360px" />
    </div>
  </div>
</template>
