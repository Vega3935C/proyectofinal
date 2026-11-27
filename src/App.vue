


<script setup>
import { ref } from 'vue'
import LoginForm from './components/LoginForm.vue'
import PerfilUsuario from './components/PerfilUsuario.vue'
import ListaItems from './components/ListaItems.vue'
import DetalleItem from './components/DetalleItem.vue'

const token = ref(localStorage.getItem('token'))

// Guardar el ID del item seleccionado
const itemSeleccionado = ref(null)

// Recibir el aviso de inicio de sesión
function alIniciarSesion(nuevoToken) {
  token.value = nuevoToken
}

// Cerrar sesión
function alCerrarSesion() {
  localStorage.removeItem('token')
  token.value = null
  itemSeleccionado.value = null
}

// Mostrar el detalle del item seleccionado
function verDetalle(id) {
  itemSeleccionado.value = id
}

// Regresar al listado
function volverLista() {
  itemSeleccionado.value = null
}
</script>

<template>
  <template v-if="token">
    <PerfilUsuario @sesion-cerrada="alCerrarSesion" />

    <DetalleItem
      v-if="itemSeleccionado !== null"
      :id="itemSeleccionado"
      @volver="volverLista"
    />

    <ListaItems
      v-else
      @ver-detalle="verDetalle"
    />
  </template>

  <LoginForm
    v-else
    @sesion-iniciada="alIniciarSesion"
  />
</template>
