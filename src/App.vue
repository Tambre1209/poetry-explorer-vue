<script setup>
import { ref, reactive } from 'vue'
import Sidebar from './components/Sidebar.vue'
import Search from './components/Search.vue'

//Central Application State
const currentView = ref('search') // Available views: search, poem, about
const apiCache = reactive ({})
const selectedPoem = ref(null)

// Search State
const currentSearch = reactive ({
    query: '',
    type: 'title',
    results: []
})

// Naviagtion Handlers
const navigateTo = (view) => {
    currentView.value = view
}

const openPoem = (poem) => {
    selectedPoem.value = poem
    currentView.value = 'poem'
}

const fetchRandom = async () => {
    // API logic will go here later
    console.log('Random poem requested')
}
</script>

<template>
    <div class="flex flex-col md:flex-row min-h-screen w-full bg-[#FCF5E5] overflow-x-hidden">

        <Sidebar @navigate="navigateTo" @fetch-random="fetchRandom" />

        <main class="flex-1 bg-[#FBF9F4] md:rounded-l-3xl relative overflow-hidden" style="box-shadow: -15px 0 45px -5px rgba(0,0,0,0.08);">
            <div class="max-w-3xl mx-auto w-full px-5 md:px-20 py-10 md:py-16">

                <div v-if="currentView === 'search'">
                    <Search
                        :currentSearch="currentSearch"
                        :apiCache="apiCache"
                        @open-poem="openPoem"
                    />
                </div>

                <div v-else-if="currentView === 'poem'">
                    <p>Poem Detail Placeholder</p>
                </div>

                <div v-else-if="currentView === 'about'">
                    <p>About View Placeholder</p>
                </div>

            </div>
        </main>

    </div>
</template>
