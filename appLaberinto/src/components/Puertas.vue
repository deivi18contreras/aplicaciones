<template>
  <div :style="{ backgroundColor: color }" class="puerta">
    <p>{{ estado }}</p>
    <button @click="abrirCerrar">
      {{ estado === 'abierta' ? 'Cerrar' : 'Abrir' }} 
    </button>
  </div>
</template>

<script setup>
const props = defineProps({
  estado: {
    type: String,
    required: true
  },
  color: {
    type: String,
    default: 'brown'
  }
})

const emit = defineEmits(['cambiar-estado'])

function abrirCerrar() {
  const nuevoEstado = props.estado === 'abierta' ? 'cerrada' : 'abierta'
  emit('cambiar-estado', nuevoEstado)
}
</script>


