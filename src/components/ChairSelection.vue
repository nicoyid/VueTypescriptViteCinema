<script setup lang="ts">
import { ref, computed } from 'vue'

const props = defineProps<{
  movie: { id: number; title: string; time: string }
}>()

const emit = defineEmits(['updateChairs'])

const chairs = ref(Array(50).fill(false))

const selectedChairs = computed(() => {
  return chairs.value.reduce((acc: number[], chair, index) => {
    if (chair) acc.push(index + 1)
    return acc
  }, [])
})

const toggleChair = (index: number) => {
  chairs.value[index] = !chairs.value[index]
}

const totalPrice = computed(() => selectedChairs.value.length * 10)

const proceedToAuth = () => {
  if (selectedChairs.value.length > 0) {
    emit('updateChairs', selectedChairs.value)
  } else {
    alert('Por favor, seleccione al menos un asiento.')
  }
}
</script>

<template>
  <div class="chair-selection">
    <h2>{{ movie.title }} - {{ movie.time }}</h2>
    <div class="screen">Pantalla</div>
    <div class="chairs">
      <button
        v-for="(chair, index) in chairs"
        :key="index"
        :class="{ selected: chair }"
        @click="toggleChair(index)"
      >
        {{ index + 1 }}
      </button>
    </div>
    <div class="summary">
      <p>Asientos seleccionados: {{ selectedChairs.join(', ') }}</p>
      <p>Precio total: ${{ totalPrice }}</p>
    </div>
    <button @click="proceedToAuth" class="proceed-button">Proceder al Pago</button>
  </div>
</template>

<style scoped>
.chair-selection {
  text-align: center;
}
.screen {
  background-color: #ccc;
  padding: 10px;
  margin-bottom: 20px;
}
.chairs {
  display: grid;
  grid-template-columns: repeat(10, 1fr);
  gap: 10px;
  margin-bottom: 20px;
}
.chairs button {
  width: 40px;
  height: 40px;
  background-color: #4CAF50;
  border: none;
  color: white;
  cursor: pointer;
}
.chairs button.selected {
  background-color: #f44336;
}
.summary {
  margin-top: 20px;
}
.proceed-button {
  margin-top: 20px;
  padding: 10px 20px;
  background-color: #008CBA;
  color: white;
  border: none;
  cursor: pointer;
}
</style>