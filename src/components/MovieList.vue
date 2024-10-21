<script setup lang="ts">
import { ref } from 'vue'

interface Movie {
  id: number;
  title: string;
  description: string;
  image: string;
  showings: { time: string; room: string }[];
}

const movies = ref<Movie[]>([
  {
    id: 1,
    title: 'Inception',
    description: 'A thief who enters the dreams of others to steal secrets from their subconscious.',
    image: 'https://m.media-amazon.com/images/M/MV5BMjAxMzY3NjcxNF5BMl5BanBnXkFtZTcwNTI5OTM0Mw@@._V1_SX300.jpg',
    showings: [
      { time: '19:00', room: 'Sala 1' },
      { time: '21:30', room: 'Sala 2' }
    ]
  },
  {
    id: 2,
    title: 'The Dark Knight',
    description: 'When the menace known as the Joker wreaks havoc and chaos on the people of Gotham, Batman must accept one of the greatest psychological and physical tests of his ability to fight injustice.',
    image: 'https://m.media-amazon.com/images/M/MV5BMTMxNTMwODM0NF5BMl5BanBnXkFtZTcwODAyMTk2Mw@@._V1_SX300.jpg',
    showings: [
      { time: '18:30', room: 'Sala 3' },
      { time: '22:00', room: 'Sala 1' }
    ]
  },
  {
    id: 3,
    title: 'Interstellar',
    description: 'A team of explorers travel through a wormhole in space in an attempt to ensure humanity\'s survival.',
    image: 'https://m.media-amazon.com/images/M/MV5BZjdkOTU3MDktN2IxOS00OGEyLWFmMjktY2FiMmZkNWIyODZiXkEyXkFqcGdeQXVyMTMxODk2OTU@._V1_SX300.jpg',
    showings: [
      { time: '20:00', room: 'Sala 2' },
      { time: '23:00', room: 'Sala 3' }
    ]
  },
])

const emit = defineEmits(['selectMovie'])

const selectMovie = (movie: Movie, showing: { time: string; room: string }) => {
  emit('selectMovie', { ...movie, selectedTime: showing.time, selectedRoom: showing.room })
}
</script>

<template>
  <div class="movie-list">
    <h2>Now Showing</h2>
    <div class="movies">
      <div v-for="movie in movies" :key="movie.id" class="movie-card">
        <img :src="movie.image" :alt="movie.title" class="movie-image">
        <div class="movie-info">
          <h3>{{ movie.title }}</h3>
          <p>{{ movie.description }}</p>
          <div class="showings">
            <button 
              v-for="showing in movie.showings" 
              :key="showing.time"
              @click="selectMovie(movie, showing)"
              class="showing-button"
            >
              {{ showing.time }} - {{ showing.room }}
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.movie-list {
  max-width: 1200px;
  margin: 0 auto;
}

.movies {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 20px;
}

.movie-card {
  border: 1px solid #ddd;
  border-radius: 8px;
  overflow: hidden;
  transition: transform 0.3s ease;
}

.movie-card:hover {
  transform: translateY(-5px);
}

.movie-image {
  width: 100%;
  height: 400px;
  object-fit: cover;
}

.movie-info {
  padding: 15px;
}

.movie-info h3 {
  margin-top: 0;
}

.showings {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  margin-top: 10px;
}

.showing-button {
  background-color: #4CAF50;
  color: white;
  border: none;
  padding: 8px 12px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.showing-button:hover {
  background-color: #45a049;
}
</style>