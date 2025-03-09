<template>
  <v-app>
    <v-progress-circle
      v-if="loading"
      indeterminate
      color="pink"
      size="50"
      class="my-4"
    ></v-progress-circle>

    <Drawer ref="drawerComponent" />

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
const loading = ref(true);

const toggleDrawer = () => {
  if (drawerComponent.value) {
    drawerComponent.value.toggle();
  }
};

onMounted(() => {
  setTimeout(() => {
    loading.value = false;
  }, 2000);
});
</script>

<style>
.image-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: calc(100vh - 64px);
  width: 100%;
  margin-top: 44px;
  transition: margin-left 0.3s ease;
}
</style>
