<script setup>
import { ref, onMounted } from 'vue'

const imageUrl = ref('')
const loading = ref(true)

async function fetchImage() {
  loading.value = true
  const response = await fetch('https://picsum.photos/800/500')
  imageUrl.value = response.url
  loading.value = false
}

// Llamar a fetchImage cuando el componente se monta
onMounted(fetchImage)
</script>

<template>
  <div class="image-container">
    <v-progress-linear v-if="loading" indeterminate color="primary" height="4" class="progress-bar"></v-progress-linear>
    <img v-else :src="imageUrl" alt="Random Image" class="responsive-image" />
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
