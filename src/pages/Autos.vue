<template>
  <v-app>
    <!-- Drawer -->
    <Drawer ref="drawerComponent" />

    <v-app-bar app>
      <v-app-bar-nav-icon @click="toggleDrawer"></v-app-bar-nav-icon>
      <v-toolbar-title>Mi Aplicación</v-toolbar-title>
    </v-app-bar>

    <!-- Contenido principal -->
    <v-main>
      <v-container>
        <!-- Llamamos al componente Tabla.vue -->
        <Tabla />
      </v-container>
    </v-main>
  </v-app>
</template>

<script setup>
import { ref } from 'vue';
import Drawer from '@/components/drawer.vue';
import Tabla from '@/components/tablaAutos.vue';

const drawerComponent = ref(null);

const toggleDrawer = () => {
  if (drawerComponent.value) {
    drawerComponent.value.toggle();
  }
};
</script>

<style scoped>
/* Agrega algunos estilos si es necesario */
</style>
