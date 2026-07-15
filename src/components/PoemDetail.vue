<script setup>
import { computed } from 'vue'

const props = defineProps({
  poem: Object,
  results: Array, // Used to find prev/next
  currentSearch: Object
})

const emit = defineEmits(['navigate', 'open-poem'])

// Logic to find neighbors
const currentIndex = computed(() => {
  return props.results.findIndex(p => p.title === props.poem.title && p.author === props.poem.author)
})

const prevPoem = computed(() => currentIndex.value > 0 ? props.results[currentIndex.value - 1] : null)
const nextPoem = computed(() => currentIndex.value < props.results.length - 1 ? props.results[currentIndex.value + 1] : null)
</script>

<template>
  <div class="animate-fade-in space-y-8">
    <!-- Navigation Controls -->
    <div v-if="results.length > 0" class="flex items-center justify-between border-b border-stone-200/40 pb-4 select-none">
      <div class="w-1/3 text-left">
        <button v-if="prevPoem" @click="emit('open-poem', prevPoem)" class="hover:opacity-80 transition-opacity text-[#2D4A3E] text-lg font-bold">
          Previous Poem
        </button>
      </div>
      <div class="w-1/3 text-center">
        <button @click="emit('navigate', 'search')" class="hover:text-[#722F37] transition-colors text-[#1C1B1A] text-sm font-bold uppercase tracking-wider border border-stone-300 rounded px-4 py-1.5 bg-stone-50/40">
          Back to Results
        </button>
      </div>
      <div class="w-1/3 text-right">
        <button v-if="nextPoem" @click="emit('open-poem', nextPoem)" class="hover:opacity-80 transition-opacity text-[#722F37] text-lg font-bold">
          Next Poem
        </button>
      </div>
    </div>

    <!-- Poem Content -->
    <div class="text-left max-w-full py-6">
      <h1 class="text-4xl md:text-5xl font-bold tracking-tight mb-3 text-[#1C1B1A]">{{ poem.title }}</h1>
      <h2 class="text-2xl text-[#605E5A] mb-2">{{ poem.author }}</h2>
      <p class="text-sm italic font-sans text-[#605E5A] tracking-wide mb-12">{{ poem.linecount }} lines</p>

      <div class="poem-body-text text-[#1C1B1A] text-left whitespace-pre-wrap selection:bg-[#722F37]/10 text-[1.2rem] leading-[1.6]">
        {{ poem.lines.join('\n') }}
      </div>
    </div>
  </div>
</template>
