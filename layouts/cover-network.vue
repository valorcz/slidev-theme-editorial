<template>
  <div class="slidev-layout cover-network h-full w-full flex flex-col justify-center px-16 relative overflow-hidden bg-[var(--slidev-theme-bg)]">
    
    <div class="absolute top-[-20%] right-[-10%] w-[600px] h-[600px] rounded-full bg-[var(--slidev-theme-primary)] opacity-10 blur-[120px]"></div>

    <div class="mb-6 relative z-10">
      <span class="px-3 py-1 rounded-full border border-[var(--slidev-theme-border)] text-xs font-mono text-[var(--slidev-theme-dim)] uppercase tracking-wider bg-[var(--slidev-theme-surface)]">
        <slot name="tag">Protocol Analysis // 2026</slot>
      </span>
    </div>

    <div class="relative z-10 max-w-4xl">
      <h1 class="text-7xl leading-tight mb-6 tracking-tight font-extrabold text-[var(--slidev-theme-color)]">
        <slot />
      </h1>
      
      <p class="text-2xl text-[var(--slidev-theme-dim)] font-light max-w-2xl leading-relaxed">
        <slot name="subtitle" />
      </p>
    </div>

    <div class="absolute bottom-12 left-16 right-16 flex justify-between items-end border-t border-[var(--slidev-theme-border)] pt-6">
      <div class="flex flex-col">
        <span class="text-xs uppercase text-[var(--slidev-theme-dim)] mb-1">Presented By</span>
        <span class="font-bold text-[var(--slidev-theme-color)]">{{ $slidev.configs.author }}</span>
      </div>
      <div class="h-3 w-3 rounded-full bg-[var(--slidev-theme-primary)] animate-pulse shadow-[0_0_10px_var(--slidev-theme-primary)]"></div>
    </div>
  </div>
</template>