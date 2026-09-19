<script setup lang="ts">
import { computed } from 'vue'
import { resolveAssetUrl } from '../layoutHelper'

const props = withDefaults(
  defineProps<{
    name: string
    role?: string
    company?: string
    photo?: string
    social?: string
    size?: 'sm' | 'md' | 'lg'
  }>(),
  {
    size: 'md',
  }
)

const initials = computed(() => {
  if (!props.name) return '?'
  return props.name
    .split(/\s+/)
    .filter(Boolean)
    .map(w => w[0])
    .slice(0, 2)
    .join('')
    .toUpperCase()
})

const photoUrl = computed(() => {
  return props.photo ? resolveAssetUrl(props.photo) : ''
})
</script>

<template>
  <div 
    class="person-card inline-flex items-center gap-4 p-4 rounded-xl border border-[var(--slidev-theme-border)] bg-[var(--slidev-theme-surface)] shadow-sm transition-all duration-200 hover:border-[var(--slidev-theme-primary)]/40 my-2"
    :class="[
      size === 'sm' ? 'p-2.5 gap-3' : size === 'lg' ? 'p-5 gap-5' : 'p-4 gap-4'
    ]"
  >
    <!-- Avatar (Image or Initials) -->
    <div 
      class="person-avatar flex-shrink-0 rounded-full border-2 border-[var(--slidev-theme-primary)]/30 overflow-hidden flex items-center justify-center font-mono font-bold bg-[var(--slidev-theme-bg)] text-[var(--slidev-theme-primary)] select-none shadow-xs"
      :class="[
        size === 'sm' ? 'w-10 h-10 text-sm' : size === 'lg' ? 'w-16 h-16 text-xl' : 'w-12 h-12 text-base'
      ]"
    >
      <img 
        v-if="photoUrl" 
        :src="photoUrl" 
        :alt="name" 
        class="w-full h-full object-cover"
      />
      <span v-else>{{ initials }}</span>
    </div>

    <!-- Metadata -->
    <div class="person-meta flex flex-col justify-center min-w-0">
      <div class="person-name font-bold text-[var(--slidev-theme-color)] leading-snug" :class="size === 'lg' ? 'text-lg' : 'text-base'">
        {{ name }}
      </div>
      <div v-if="role || company" class="person-role text-xs md:text-sm text-[var(--slidev-theme-dim)] leading-tight mt-0.5">
        <span v-if="role">{{ role }}</span>
        <span v-if="role && company" class="opacity-50 mx-1">·</span>
        <span v-if="company" class="font-medium opacity-90">{{ company }}</span>
      </div>
      <div v-if="social" class="person-social font-mono text-[0.7rem] text-[var(--slidev-theme-primary)] mt-1 opacity-80">
        {{ social }}
      </div>
      <div v-if="$slots.default" class="text-xs text-[var(--slidev-theme-dim)] mt-1.5 leading-normal">
        <slot />
      </div>
    </div>
  </div>
</template>
