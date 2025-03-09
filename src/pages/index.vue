<template>
  <v-app>
    <!-- Círculo de carga que se muestra mientras se carga la imagen -->
    <v-progress-circle
      v-if="loading"
      indeterminate
      color="pink"
      size="50"
      class="my-4"
    ></v-progress-circle>

    <!-- Drawer -->
    <Drawer ref="drawerComponent" />

    <!-- Imagen - Se muestra cuando 'loading' es false -->
    <Imagen class="image-container" v-if="!loading" />

    <v-app-bar app>
      <v-app-bar-nav-icon @click="toggleDrawer"></v-app-bar-nav-icon>
      <v-toolbar-title>Mi Aplicación</v-toolbar-title>
    </v-app-bar>

    <v-main>
      <router-view></router-view>
    </v-main>
  </v-app>
</template>

<script setup>
import { ref, onMounted } from "vue";
import Drawer from "@/components/drawer.vue";
import Imagen from "@/components/imagen.vue";

const drawerComponent = ref(null);
const loading = ref(true); // Estado de carga

const toggleDrawer = () => {
  if (drawerComponent.value) {
    drawerComponent.value.toggle();
  }
};

// Simulación de carga de datos (esto puede representar cualquier proceso de carga)
onMounted(() => {
  setTimeout(() => {
    loading.value = false; // Cambia el estado de carga después de 2 segundos
  }, 2000); // Tiempo simulado de carga
});
</script>

<style>
.image-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: calc(100vh - 64px); /* Ajuste para no tocar la barra superior */
  width: 100%;
  margin-top: 44px; /* Añadir espacio para que no toque la barra */
  transition: margin-left 0.3s ease;
}
</style>
