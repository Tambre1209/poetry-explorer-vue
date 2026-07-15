<script setup>
    defineProps({ currentView: String })
    const emit = defineEmits(['navigate', 'fetch-random'])
</script>

<template>
  <!-- Mobile Navigation -->
    <header class="bg-nav-bg px-6 py-4 border-b border-stone-200/40 flex justify-between items-center md:hidden shrink-0">
      <span class="text-xl font-bold uppercase text-ink">Poetry Explorer</span>
      <div class="flex gap-5 text-sm font-bold uppercase">
        <button @click="emit('navigate', 'search')" :class="[currentView === 'search' ? 'text-accent' : 'text-ink']">Search</button>
        <button @click="emit('fetch-random')" class="text-ink">Random</button>
        <button @click="emit('navigate', 'about')" :class="[currentView === 'about' ? 'text-accent' : 'text-ink']">About</button>
      </div>
    </header>

  <!-- Desktop Sidebar -->
  <aside class="w-72 bg-nav-bg px-10 py-12 hidden md:flex flex-col gap-12 shrink-0 h-screen sticky top-0 self-start border-r border-stone-200/40">
    <div class="flex flex-col gap-4">
      <div class="w-16 h-16 bg-accent flex items-center justify-center rounded-sm">
        <svg class="w-8 h-8 text-nav-bg" fill="none" stroke="currentColor" stroke-width="1.5" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" d="m16.862 4.487 1.687-1.688a1.875 1.875 0 1 1 2.652 2.652L6.832 19.82a4.5 4.5 0 0 1-1.897 1.13l-2.685.8.8-2.685a4.5 4.5 0 0 1 1.13-1.897L16.863 4.487Zm0 0L19.5 7.125" />
        </svg>
      </div>
      <h1 class="text-3xl font-bold tracking-tight uppercase leading-tight text-ink">
        Poetry<br>Explorer
      </h1>
    </div>

    <nav class="flex flex-col gap-6 text-xl items-start">
      <button @click="emit('navigate', 'search')" :class="[currentView === 'search' ? 'text-accent' : 'text-ink']">Search</button>
      <button @click="emit('fetch-random')" :class="[currentView === 'random' ? 'text-accent' : 'text-ink']">Random Discover</button>
      <button @click="emit('navigate', 'about')" :class="[currentView === 'about' ? 'text-accent' : 'text-ink']">About</button>
    </nav>
  </aside>
</template>
