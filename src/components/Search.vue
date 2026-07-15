<script setup>
import { ref } from 'vue'

const props = defineProps({
  currentSearch: Object,
  apiCache: Object
})

const emit = defineEmits(['open-poem'])

const isSearching = ref(false)
const apiError = ref(null)

const executeSearch = async () => {
  if (!props.currentSearch.query.trim()) return

  isSearching.value = true
  apiError.value = null
  props.currentSearch.results = []

  const url = `https://poetrydb.org/${props.currentSearch.type}/${props.currentSearch.query}`

  if (props.apiCache[url]) {
    props.currentSearch.results = props.apiCache[url]
    isSearching.value = false
    return
  }

  try {
    const response = await fetch(url)
    const data = await response.json()

    if (Array.isArray(data) && data.length > 0 && !data[0].status) {
      const sorted = data.sort((a, b) => a.title.localeCompare(b.title))
      props.apiCache[url] = sorted
      props.currentSearch.results = sorted
    } else {
      props.currentSearch.results = []
    }
  } catch (error) {
    apiError.value = 'The poetry database took too long to respond. Please try again.'
  } finally {
    isSearching.value = false
  }
}
</script>

<template>
  <div>
    <div class="mb-12 border-b border-stone-200/40 pb-8">
      <form @submit.prevent="executeSearch" class="flex flex-col sm:flex-row gap-3">
        <select v-model="currentSearch.type" class="bg-transparent border-b border-stone-400 py-1 px-2 focus:outline-none focus:border-accent text-base font-primary">
          <option value="author">Author</option>
          <option value="title">Title</option>
        </select>

        <input
          type="text"
          v-model="currentSearch.query"
          placeholder="Type poet or title name..."
          required
          class="flex-1 bg-transparent border-b border-stone-400 py-1 px-2 focus:outline-none focus:border-accent placeholder-stone-400 text-lg font-primary"
        />

        <button
          type="submit"
          :disabled="isSearching"
          class="text-sm font-bold uppercase tracking-wider text-accent hover:text-next transition-colors px-4 py-1 font-submit"
        >
          {{ isSearching ? 'Searching...' : 'Search Archive' }}
        </button>
      </form>
    </div>

    <div id="search-results-target">
      <div v-if="apiError" class="text-center py-20 border border-dashed border-stone-200 rounded bg-[#F4F0E6]/30">
        <p class="italic text-next text-lg font-bold">Network Delay</p>
        <p class="text-xs text-stone-400 mt-2 font-mono">{{ apiError }}</p>
      </div>

      <div v-else-if="currentSearch.results && currentSearch.results.length > 0" class="space-y-8 animate-fade-in">
        <div class="text-xs font-mono uppercase tracking-widest text-meta border-b border-stone-200/60 pb-2">
          Found {{ currentSearch.results.length }} matches in archive
        </div>
        
        <ul class="divide-y divide-stone-200/20">
          <li v-for="(poem, index) in currentSearch.results" :key="index" class="py-1">
            <button 
              @click="emit('open-poem', poem)"
              class="w-full text-left group flex flex-col md:flex-row md:items-baseline md:justify-between gap-2 py-3 px-4 rounded hover:bg-stone-200/50 -mx-4 transition-all duration-150"
            >
              <div class="space-y-1">
                <h3 class="text-xl font-bold text-ink group-hover:text-accent transition-colors">
                  {{ poem.title }}
                </h3>
                <p class="text-sm text-meta">
                  by {{ poem.author }}
                </p>
              </div>
              
              <span class="text-xs font-mono text-meta group-hover:text-next transition-colors whitespace-nowrap">
                {{ poem.linecount }} lines <span class="group-hover:translate-x-1 inline-block transition-transform">→</span>
              </span>
            </button>
          </li>
        </ul>
      </div>

      <div v-else-if="currentSearch.query && !isSearching && currentSearch.results.length === 0" class="text-center py-20 border border-dashed border-stone-200 rounded bg-[#F4F0E6]/30">
        <p class="italic text-meta text-lg">The archives are silent.</p>
        <p class="text-xs text-stone-400 mt-2 font-mono">No matching records found. Verify spelling or switch parameters.</p>
      </div>

      <div v-else class="text-center py-20">
        <p class="italic text-stone-400 text-lg">Select search indices above to explore the collection.</p>
      </div>
    </div>
  </div>
</template>
