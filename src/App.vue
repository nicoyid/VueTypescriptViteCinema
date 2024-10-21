<script setup lang="ts">
import { ref } from 'vue'
import MovieList from './components/MovieList.vue'
import ChairSelection from './components/ChairSelection.vue'
import UserAuth from './components/UserAuth.vue'
import Payment from './components/Payment.vue'

interface Movie {
  id: number;
  title: string;
  description: string;
  image: string;
  selectedTime: string;
  selectedRoom: string;
}

const selectedMovie = ref<Movie | null>(null)
const selectedChairs = ref<number[]>([])
const isAuthenticated = ref(false)
const currentStep = ref('movieList')

const selectMovie = (movie: Movie) => {
  selectedMovie.value = movie
  currentStep.value = 'chairSelection'
}

const updateSelectedChairs = (chairs: number[]) => {
  selectedChairs.value = chairs
  currentStep.value = 'auth'
}

const handleAuth = (success: boolean) => {
  isAuthenticated.value = success
  if (success) {
    currentStep.value = 'payment'
  }
}

const resetBooking = () => {
  selectedMovie.value = null
  selectedChairs.value = []
  isAuthenticated.value = false
  currentStep.value = 'movieList'
}
</script>

<template>
  <div class="cinema-app">
    <h1>Cinema Website</h1>
    <MovieList v-if="currentStep === 'movieList'" @select-movie="selectMovie" />
    <ChairSelection
      v-else-if="currentStep === 'chairSelection'"
      :movie="selectedMovie"
      @update-chairs="updateSelectedChairs"
    />
    <UserAuth
      v-else-if="currentStep === 'auth'"
      @auth-result="handleAuth"
    />
    <Payment
      v-else-if="currentStep === 'payment'"
      :movie="selectedMovie"
      :chairs="selectedChairs"
      @payment-complete="resetBooking"
    />
  </div>
</template>

<style>
.cinema-app {
  font-family: Arial, sans-serif;
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

body {
  background-color: #f0f0f0;
  color: #333;
}

h1, h2 {
  color: #2c3e50;
}
</style>