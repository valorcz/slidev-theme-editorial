<template>
  <div class="slidev-layout cover h-full w-full flex flex-col justify-center px-16 relative overflow-hidden">
    
    <div class="absolute top-[-20%] right-[-10%] w-[500px] h-[500px] rounded-full bg-[var(--slidev-theme-primary)] opacity-10 blur-[100px]"></div>

    <div class="mb-6">
      <span class="px-3 py-1 rounded-full border border-[var(--slidev-theme-border)] text-xs font-mono text-[var(--slidev-theme-dim)] uppercase tracking-wider bg-[var(--slidev-theme-surface)]">
        {{ $slidev.configs.date || 'Presentation' }}
      </span>
    </div>

    <div class="relative z-10 max-w-4xl">
      <h1 class="text-6xl md:text-7xl leading-tight mb-6 tracking-tight">
        <slot />
      </h1>
      
      <p class="text-2xl text-[var(--slidev-theme-dim)] font-light max-w-2xl leading-relaxed">
        <slot name="subtitle" />
      </p>
    </div>

    <div class="absolute bottom-12 left-16 right-16 flex justify-between items-end border-t border-[var(--slidev-theme-border)] pt-6">
      <div class="flex flex-col">
        <span class="text-xs uppercase text-[var(--slidev-theme-dim)] mb-1">Presented By</span>
        <span class="font-bold">{{ $slidev.configs.author || 'Author' }}</span>
      </div>
      <div class="h-8 w-8 rounded-full bg-[var(--slidev-theme-primary)]"></div>
    </div>
    
  </div>
</template>