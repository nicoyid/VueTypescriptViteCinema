<script setup lang="ts">
import { computed } from 'vue'

const props = defineProps<{
  movie: { id: number; title: string; time: string },
  chairs: number[]
}>()

const emit = defineEmits(['paymentComplete'])

const totalPrice = computed(() => props.chairs.length * 10)

const processPayment = () => {
  // Aquí iría la lógica real de procesamiento de pago
  alert('Pago procesado con éxito')
  emit('paymentComplete')
}
</script>

<template>
  <div class="payment">
    <h2>Pago</h2>
    <p>Película: {{ movie.title }} - {{ movie.time }}</p>
    <p>Asientos seleccionados: {{ chairs.join(', ') }}</p>
    <p>Total a pagar: ${{ totalPrice }}</p>
    <button @click="processPayment">Realizar Pago</button>
  </div>
</template>

<style scoped>
.payment {
  max-width: 400px;
  margin: 0 auto;
}
button {
  width: 100%;
  padding: 10px;
  background-color: #4CAF50;
  color: white;
  border: none;
  cursor: pointer;
  margin-top: 20px;
}
</style>