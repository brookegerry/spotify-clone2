<template>
    <div class="p-8">
        <!-- Search Bar -->
        <div class="search-bar">
            <input v-model="searchQuery" placeholder="Search for artists, albums, or songs" />
            <button @click="search">Search</button>
        </div>

        <!-- Search Results -->
        <div v-if="searchResults.length" class="search-results">
            <h2>Search Results:</h2>
            <ul>
                <li v-for="result in searchResults" :key="result.id">{{ result.name }}</li>
            </ul>
        </div>

        <!-- Categories -->
        <button
            type="button"
            class="text-white text-2xl font-semibold hover:underline cursor-pointer"
        >
            Browse all
        </button>
        <div class="py-1.5"></div>
        <div class="grid xl:grid-cols-5 lg:grid-cols-4 md:grid-cols-3 sm:grid-cols-2 gap-6">
            <!-- ... (your categories code) -->
        </div>
    </div>
</template>


<script setup>
import { ref } from 'vue';

// Define the search query and results
const searchQuery = ref('');
const searchResults = ref([]);
// vars ??
// Function to handle the search
async function search() {
    try {
        const response = await fetch(`http://search-service.nakatomi.svc.cluster.local/api/search?term=${searchQuery.value}`);
        if (response.ok) {
            const data = await response.json();
            searchResults.value = data;
        } else {
            console.error('Failed to fetch search results');
        }
    } catch (error) {
        console.error('Error:', error);
    }
    if (searchResults.length==0) {
        searchResults.value = 'not found'
        }
    }

</script>

<style scoped>
/* Search Bar Styles */
.search-bar {
    display: flex;
    align-items: center;
    margin-bottom: 20px;
}

input {
    padding: 10px;
    border: 1px solid #ccc;
    width: 300px;
    border-radius: 5px;
    margin-right: 10px;
}

button {
    padding: 10px 20px;
    background-color: #333;
    color: #fff;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

button:hover {
    background-color: #555;
}

/* Search Results Styles (if you need any additional styling) */
.search-results {
    margin-bottom: 20px;
    color: white;  /* This makes the text color white */
}
</style>
