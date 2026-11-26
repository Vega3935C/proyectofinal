
<script setup>
import { ref } from 'vue'
import LoginForm from './components/LoginForm.vue'
import PerfilUsuario from './components/PerfilUsuario.vue'

// Leer el token guardado al abrir la aplicación
const token = ref(localStorage.getItem('token'))

// Recibir el aviso cuando el usuario inicia sesión
function alIniciarSesion(nuevoToken) {
  token.value = nuevoToken
}

// Cerrar sesión y regresar al formulario
function alCerrarSesion() {
  localStorage.removeItem('token')
  token.value = null
}
</script>

<template>
  <PerfilUsuario
    v-if="token"
    @sesion-cerrada="alCerrarSesion"
  />

  <LoginForm
    v-else
    @sesion-iniciada="alIniciarSesion"
  />
</template>
