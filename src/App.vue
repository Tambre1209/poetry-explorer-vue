<script setup>
import { ref, reactive } from 'vue'
import Sidebar from './components/Sidebar.vue'
import Search from './components/Search.vue'
import PoemDetail from './components/PoemDetail.vue'
import About from './components/About.vue'

//Central Application State
const currentView = ref('search') // Available views: search, poem, about
const apiCache = reactive ({})
const selectedPoem = ref(null)
const isLoading = ref(false) // Global loading state

// Search State
const currentSearch = reactive ({
    query: '',
    type: 'title',
    results: []
})

// Naviagtion Handlers
const navigateTo = (view) => {
    currentView.value = view
    window.scrollTo(0, 0)
}

const openPoem = (poem) => {
    selectedPoem.value = poem
    currentView.value = 'poem'
    window.scrollTo(0, 0)
}

// API Error
const apiError = ref(null)

// Random Poem Fetcher
const fetchRandom = async () => {
    isLoading.value = true
    apiError.value = null
    try {
        const response = await fetch('https://poetrydb.org/random/1')
        const data = await response.json()

        if (Array.isArray(data) && data.length > 0) {
            selectedPoem.value = data[0]
            currentView.value = 'random' // only set once poem data actually exists
            currentSearch.results = []
        } else {
            throw new Error('No poem found')
        }
    } catch (error) {
        apiError.value = 'Failed to load random poem. Please try again.'
    } finally {
        isLoading.value = false
    }
}
</script>

<template>
    <div class="flex flex-col md:flex-row min-h-screen w-full bg-nav-bg">
        
        <div v-show="isLoading" class="fixed top-0 left-0 w-full h-1.5 z-50">
            <div class="h-full bg-accent animate-pulse w-full"></div>
        </div>

        <Sidebar 
          :currentView="currentView" 
          @navigate="navigateTo" 
          @fetch-random="fetchRandom" 
        />


        <main class="flex-1 bg-poem-bg md:rounded-l-3xl relative overflow-hidden" style="box-shadow: -15px 0 45px -5px rgba(0,0,0,0.08);">
            <div class="max-w-3xl mx-auto w-full px-5 md:px-20 py-10 md:py-16">
                
                <div v-if="apiError" class="text-center py-20 text-next font-bold">{{ apiError }}</div>

                <div v-if="currentView === 'search'">
                    <Search
                        :currentSearch="currentSearch"
                        :apiCache="apiCache"
                        @open-poem="openPoem"
                        @set-loading="(status) => isLoading = status"
                    />
                </div>

                <div v-else-if="currentView === 'poem' || currentView === 'random'">
                    <PoemDetail
                        :poem="selectedPoem"
                        :results="currentSearch.results"
                        :currentSearch="currentSearch"
                        @navigate="navigateTo"
                        @open-poem="openPoem"
                    />
                </div>

                <div v-else-if="currentView === 'about'">
                    <About />
                </div>

            </div>
        </main>

    </div>
</template>
