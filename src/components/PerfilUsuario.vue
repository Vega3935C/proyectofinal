
<script setup>
import { ref, onMounted } from 'vue'
import { obtenerPerfil } from '../services/api.js'

// Evento para avisar a App.vue que el usuario quiere salir
const emit = defineEmits(['sesion-cerrada'])

const perfil = ref(null)
const cargando = ref(true)
const error = ref('')

// Consultar los datos del perfil
async function cargarPerfil() {
  cargando.value = true
  error.value = ''

  try {
    perfil.value = await obtenerPerfil()
    console.log('Perfil del usuario:', perfil.value)

  } catch (e) {
    error.value = e.message

  } finally {
    cargando.value = false
  }
}

// Cerrar sesión
function cerrarSesion() {
  emit('sesion-cerrada')
}

// Consultar el perfil cuando aparece el componente
onMounted(cargarPerfil)
</script>

<template>
  <h1>Mi perfil</h1>

  <p v-if="cargando">Cargando perfil...</p>

  <p v-else-if="error">
    Error: {{ error }}
  </p>

  <div v-else-if="perfil">
    <p>
      <strong>Nombre:</strong>
      {{ perfil.first_name }} {{ perfil.last_name }}
    </p>

    <p>
      <strong>Correo:</strong>
      {{ perfil.email }}
    </p>

    <p>
      <strong>Escuela:</strong>
      {{ perfil.school }}
    </p>

    <p>
      <strong>Items:</strong>
      {{ perfil.items_count }} de {{ perfil.items_limit }}
    </p>
  </div>

  <button @click="cargarPerfil" :disabled="cargando">
    Actualizar
  </button>

  <button @click="cerrarSesion">
    Cerrar sesión
  </button>
</template>
