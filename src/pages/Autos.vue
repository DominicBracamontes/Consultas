<template>
  <v-app>
    <Drawer ref="drawerComponent" />

    <v-app-bar app>
      <v-app-bar-nav-icon @click="toggleDrawer"></v-app-bar-nav-icon>
      <v-toolbar-title>Mi Aplicación</v-toolbar-title>
    </v-app-bar>

    <v-main>
      <v-container>
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
</style>
