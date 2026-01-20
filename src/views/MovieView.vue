<!-- src/views/MovieView.vue -->
<script setup>
import { ref, onMounted } from 'vue';
import { useRoute, useRouter } from 'vue-router';
import axios from 'axios';

const route = useRoute();
const router = useRouter();
const movie = ref(null);
const loading = ref(true);

const API_BASE = import.meta.env.VITE_API_BASE_URL;

onMounted(async () => {
  try {
    const res = await axios.get(`${API_BASE}/${route.params.id}`);
    movie.value = res.data;
  } catch (err) {
    console.error(err);
  } finally {
    loading.value = false;
  }
});

const imageError = (e) => {
  e.target.src = 'https://via.placeholder.com/300x450?text=No+Poster';
};
</script>

<template>
  <div class="container movie-detail">
    <button class="back-btn" @click="router.back()">← Back</button>
    
    <div v-if="loading">Loading details...</div>
    
    <div v-else-if="movie" class="content">
      <div class="poster-wrapper">
        <img 
          :src="movie.poster || 'https://via.placeholder.com/300x450?text=No+Poster'" 
          @error="imageError"
          alt="Poster" 
        />
      </div>
      
      <div class="details">
        <h1>{{ movie.title }} <span class="year">({{ movie.year }})</span></h1>
        
        <div class="tags">
          <span v-for="genre in movie.genres" :key="genre" class="tag">{{ genre }}</span>
        </div>

        <div class="meta">
          <p v-if="movie.imdb"><strong>IMDB:</strong> {{ movie.imdb.rating }} / 10</p>
          <p v-if="movie.runtime"><strong>Runtime:</strong> {{ movie.runtime }} mins</p>
          <p v-if="movie.rated"><strong>Rated:</strong> {{ movie.rated }}</p>
        </div>

        <p class="plot">{{ movie.fullplot || movie.plot }}</p>

        <div class="cast" v-if="movie.cast">
          <h3>Cast</h3>
          <p>{{ movie.cast.join(', ') }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.movie-detail {
  padding-top: 20px;
}
.back-btn {
  background: transparent;
  color: var(--light);
  border: 1px solid var(--gray);
  padding: 5px 15px;
  cursor: pointer;
  margin-bottom: 20px;
}
.content {
  display: flex;
  flex-direction: column;
  gap: 30px;
}
@media (min-width: 768px) {
  .content {
    flex-direction: row;
  }
}
.poster-wrapper img {
  max-width: 300px;
  width: 100%;
  border-radius: 8px;
  box-shadow: 0 4px 15px rgba(0,0,0,0.5);
}
.details h1 {
  margin-top: 0;
  color: var(--primary);
}
.details .year {
  color: var(--light);
  font-size: 0.8em;
}
.tags {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}
.tag {
  background: #333;
  padding: 4px 10px;
  border-radius: 15px;
  font-size: 0.9rem;
}
.meta p {
  margin: 5px 0;
  color: var(--gray);
}
.plot {
  line-height: 1.6;
  font-size: 1.1rem;
  margin: 20px 0;
}
</style>