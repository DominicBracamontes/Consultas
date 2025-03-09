<template>
    <v-navigation-drawer v-model="drawer" app>
      <v-list>
        <!-- Espacio reservado para el usuario sin contenido -->
        <v-list-item>
          <v-list-item-title></v-list-item-title>
        </v-list-item>
      </v-list>
  
      <v-divider></v-divider>
  
      <v-list density="compact">
        <v-list-item v-for="(item, index) in menuItems" :key="index" :to="item.to">
          <template v-slot:prepend>
            <v-icon>{{ item.icon }}</v-icon>
          </template>
          <v-list-item-title>{{ item.title }}</v-list-item-title>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
  </template>
  
  <script setup>
  import { ref, defineExpose } from "vue";
  
  const drawer = ref(false);
  
  const menuItems = ref([
    { title: "Home", icon: "mdi-home", to: "/" },
    { title: "Autos", icon: "mdi-car", to: "/autos" },
  ]);
  
  const toggle = () => {
    drawer.value = !drawer.value;
  };
  
  defineExpose({ toggle });
  </script>
  