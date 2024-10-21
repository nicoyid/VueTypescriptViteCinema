<script setup lang="ts">
import { ref } from 'vue'
import { VueRecaptcha } from 'vue-recaptcha'

const emit = defineEmits(['authResult'])

const username = ref('')
const password = ref('')
const recaptchaToken = ref('')

const login = () => {
  if (!recaptchaToken.value) {
    alert('Por favor, complete el reCAPTCHA')
    return
  }
  // Simulación de autenticación
  if (username.value === 'user' && password.value === 'password') {
    emit('authResult', true)
  } else {
    alert('Credenciales incorrectas. Intente con user/password')
  }
}

const onRecaptchaVerified = (response: string) => {
  recaptchaToken.value = response
}

const onRecaptchaExpired = () => {
  recaptchaToken.value = ''
}
</script>

<template>
  <div class="user-auth">
    <h2>Iniciar Sesión</h2>
    <form @submit.prevent="login">
      <div>
        <label for="username">Usuario:</label>
        <input id="username" v-model="username" type="text" required>
      </div>
      <div>
        <label for="password">Contraseña:</label>
        <input id="password" v-model="password" type="password" required>
      </div>
      <div class="recaptcha-container">
        <vue-recaptcha
          sitekey="6LeIxAcTAAAAAJcZVRqyHh71UMIEGNQ_MXjiZKhI"
          @verify="onRecaptchaVerified"
          @expired="onRecaptchaExpired"
        ></vue-recaptcha>
      </div>
      <button type="submit">Iniciar Sesión</button>
    </form>
  </div>
</template>

<style scoped>
.user-auth {
  max-width: 300px;
  margin: 0 auto;
}
form div {
  margin-bottom: 10px;
}
label {
  display: block;
  margin-bottom: 5px;
}
input {
  width: 100%;
  padding: 5px;
}
button {
  width: 100%;
  padding: 10px;
  background-color: #4CAF50;
  color: white;
  border: none;
  cursor: pointer;
  margin-top: 10px;
}
.recaptcha-container {
  display: flex;
  justify-content: center;
  margin-top: 10px;
}
</style>