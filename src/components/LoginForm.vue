

<script setup>
import { ref } from 'vue'
import { obtenerToken } from '../services/api.js'

const correo = ref('')
const licencia = ref('')
const mensaje = ref('')
const cargando = ref(false)

async function iniciarSesion() {
  mensaje.value = ''
  cargando.value = true

  try {
    const respuesta = await obtenerToken(
      correo.value.trim(),
      licencia.value.trim()
    )

    console.log('Autenticación exitosa:', respuesta)

    // Guardar el token en el navegador
    localStorage.setItem('token', respuesta.token)

    mensaje.value = 'Sesión iniciada. Token guardado.'

  } catch (error) {
    console.error('Error al iniciar sesión:', error)
    mensaje.value = 'Error: ' + error.message

  } finally {
    cargando.value = false
  }
}

// Función para cerrar sesión
function cerrarSesion() {
  localStorage.removeItem('token')
  mensaje.value = 'Sesión cerrada.'
}
</script>

<template>
  <h1>Acceso a la API</h1>

  <form @submit.prevent="iniciarSesion">
    <label>Correo</label>
    <input v-model="correo" type="email" required />

    <label>Licencia</label>
    <input v-model="licencia" type="password" required />

    <button type="submit" :disabled="cargando">
      {{ cargando ? 'Conectando...' : 'Obtener token' }}
    </button>
  </form>

  <button @click="cerrarSesion">
    Cerrar sesión
  </button>

  <p>{{ mensaje }}</p>
</template>
