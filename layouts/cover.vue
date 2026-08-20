<template>
  <div class="slidev-layout cover h-full w-full flex flex-col justify-center px-16 relative overflow-hidden bg-[var(--slidev-theme-bg)]">
    <div class="absolute top-[-20%] right-[-10%] w-[550px] h-[550px] rounded-full bg-[var(--slidev-theme-primary)] opacity-10 blur-[110px] pointer-events-none"></div>

    <div class="mb-6 relative z-10">
      <span class="px-3 py-1 rounded-full border border-[var(--slidev-theme-border)] text-xs font-mono text-[var(--slidev-theme-dim)] uppercase tracking-wider bg-[var(--slidev-theme-surface)]">
        <slot name="tag">{{ $slidev.configs.date || 'Presentation' }}</slot>
      </span>
    </div>

    <div class="relative z-10 max-w-4xl">
      <h1 class="text-6xl md:text-7xl leading-tight mb-4 tracking-tight font-extrabold text-[var(--slidev-theme-color)]">
        <slot />
      </h1>
      
      <p class="text-2xl text-[var(--slidev-theme-dim)] font-light max-w-2xl leading-relaxed">
        <slot name="subtitle" />
      </p>
    </div>

    <div class="absolute bottom-10 left-16 right-16 flex justify-between items-end border-t border-[var(--slidev-theme-border)] pt-4">
      <div class="flex flex-col">
        <span class="text-xs uppercase text-[var(--slidev-theme-dim)] mb-0.5 font-mono">Presented By</span>
        <span class="font-bold text-[var(--slidev-theme-color)]">{{ $slidev.configs.author || 'Author' }}</span>
      </div>
      <div class="h-3 w-3 rounded-full bg-[var(--slidev-theme-primary)] animate-pulse shadow-[0_0_10px_var(--slidev-theme-primary)]"></div>
    </div>
  </div>
</template>