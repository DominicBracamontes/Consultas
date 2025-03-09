<script setup>
import { ref } from 'vue'

const imageUrl = ref('https://picsum.photos/800/500') // Imagen inicial
const loading = ref(true) // Estado para controlar si la imagen está cargando
const errorMessage = ref('') // Mensaje de error

// Función para obtener una nueva imagen
async function fetchImage() {
  loading.value = true; // Comienza a cargar la imagen
  errorMessage.value = ''; // Limpia cualquier mensaje de error anterior
  try {
    const response = await fetch('https://picsum.photos/800/500')
    if (!response.ok) {
      throw new Error('Error al cargar la imagen.')
    }
    imageUrl.value = response.url // Asigna la nueva imagen
  } catch (error) {
    errorMessage.value = 'No se pudo cargar la imagen. Intenta de nuevo.'
  } finally {
    loading.value = false; // Termina la carga, incluso si hubo error
  }
}
</script>

<template>
  <div class="image-container">
    <!-- Mostrar el indicador de carga mientras la imagen se está cargando -->
    <v-progress-circle v-if="loading" indeterminate color="blue" size="60"></v-progress-circle>

    <!-- Mostrar imagen cuando se haya cargado -->
    <img v-if="!loading" :src="imageUrl" alt="Random Image" class="responsive-image" />

    <!-- Mostrar mensaje de error si no se pudo cargar la imagen -->
    <v-alert v-if="errorMessage" type="error" dismissible>
      {{ errorMessage }}
    </v-alert>

    <button @click="fetchImage">Actualizar Imagen</button>
  </div>
</template>

<style scoped>
.image-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100vh;
  width: 100%;
}

.responsive-image {
  max-width: 80%;
  height: auto;
  border-radius: 10px;
  margin-top: 20px;
}

button {
  margin-top: 20px;
  padding: 10px 20px;
  border: none;
  background-color: #007bff;
  color: white;
  font-size: 16px;
  border-radius: 8px;
  cursor: pointer;
  box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.2);
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #0056b3;
}
</style>
