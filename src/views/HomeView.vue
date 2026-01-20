<!-- src/views/HomeView.vue -->
<script setup>
import { ref, onMounted, watch } from 'vue';
import axios from 'axios';
import { useRouter } from 'vue-router';

const router = useRouter();
const movies = ref([]);
const loading = ref(false);
const searchQuery = ref('');
const currentPage = ref(1);
const totalPages = ref(0);

// YOUR LIVE API URL
const API_BASE = import.meta.env.VITE_API_BASE_URL;

// Fetch function
const fetchMovies = async (page = 1) => {
  loading.value = true;
  try {
    const res = await axios.get(`${API_BASE}?page=${page}&limit=12`);
    movies.value = res.data.data;
    currentPage.value = res.data.page;
    totalPages.value = res.data.totalPages;
  } catch (err) {
    console.error(err);
  } finally {
    loading.value = false;
  }
};

// Search function
const searchMovies = async () => {
  if (!searchQuery.value) return fetchMovies(); // Reset if empty
  
  loading.value = true;
  try {
    const res = await axios.get(`${API_BASE}/search/${searchQuery.value}`);
    movies.value = res.data; // Search endpoint returns array directly based on your backend logic
    totalPages.value = 0; // Disable pagination on search
  } catch (err) {
    console.error(err);
  } finally {
    loading.value = false;
  }
};

// Navigate to details
const goToMovie = (id) => {
  router.push(`/movie/${id}`);
};

// Helper for broken images (Common in sample_mflix)
const imageError = (e) => {
  e.target.src = 'https://via.placeholder.com/300x450?text=No+Poster';
};

onMounted(() => fetchMovies());
</script>

<template>
  <div class="container home">
    <!-- Search Bar -->
    <div class="search-bar">
      <input 
        v-model="searchQuery" 
        @keyup.enter="searchMovies"
        placeholder="Search movies (e.g. Batman)..." 
      />
      <button @click="searchMovies">Search</button>
    </div>

    <!-- Loading State -->
    <div v-if="loading" class="loading">Loading movies...</div>

    <!-- Movie Grid -->
    <div v-else class="movie-grid">
      <div 
        v-for="movie in movies" 
        :key="movie._id" 
        class="movie-card" 
        @click="goToMovie(movie._id)"
      >
        <img 
          :src="movie.poster || 'https://via.placeholder.com/300x450?text=No+Poster'" 
          @error="imageError"
          alt="Poster" 
        />
        <div class="info">
          <h3>{{ movie.title }}</h3>
          <span class="year">{{ movie.year }}</span>
          <span class="rating" v-if="movie.imdb">⭐ {{ movie.imdb.rating }}</span>
        </div>
      </div>
    </div>

    <!-- Pagination (Only show if not searching) -->
    <div v-if="totalPages > 0 && !loading" class="pagination">
      <button 
        :disabled="currentPage === 1" 
        @click="fetchMovies(currentPage - 1)"
      >Prev</button>
      <span>Page {{ currentPage }} of {{ totalPages }}</span>
      <button 
        :disabled="currentPage === totalPages" 
        @click="fetchMovies(currentPage + 1)"
      >Next</button>
    </div>
  </div>
</template>

<style scoped>
.search-bar {
  margin: 20px 0;
  display: flex;
  gap: 10px;
}
input {
  flex: 1;
  padding: 10px;
  border-radius: 4px;
  border: none;
  font-size: 16px;
}
button {
  padding: 10px 20px;
  background: var(--primary);
  color: white;
  border: none;
  cursor: pointer;
  border-radius: 4px;
}
button:disabled {
  background: var(--gray);
  cursor: not-allowed;
}
.movie-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 20px;
}
.movie-card {
  background: #222;
  border-radius: 8px;
  overflow: hidden;
  transition: transform 0.2s;
  cursor: pointer;
}
.movie-card:hover {
  transform: scale(1.03);
}
.movie-card img {
  width: 100%;
  height: 300px;
  object-fit: cover;
}
.info {
  padding: 10px;
}
.info h3 {
  margin: 0;
  font-size: 1rem;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
.year, .rating {
  font-size: 0.8rem;
  color: var(--gray);
  margin-right: 10px;
}
.pagination {
  margin-top: 30px;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 15px;
  padding-bottom: 30px;
}
</style>