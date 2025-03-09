<template>
    <v-container>
      <v-progress-circle v-if="loading" indeterminate color="blue" size="60"></v-progress-circle>
  
      <v-data-table
        v-else
        :headers="headers"
        :items="autos"
        item-value="id"
        hide-default-footer
        class="scrollable-table"
      >
        <template v-slot:item.actions="{ item }">
          <v-icon @click="editItem(item)">mdi-pencil</v-icon>
          <v-icon color="red" @click="confirmDeleteItem(item)">mdi-delete</v-icon>
        </template>
      </v-data-table>
  
      <v-dialog v-model="editDialog" max-width="500px">
        <v-card>
          <v-card-title class="headline">Editar Auto</v-card-title>
          <v-card-text>
            <v-form ref="form" v-model="valid">
              <v-text-field
                v-model="editedItem.id"
                label="ID"
                :rules="[rules.required]"
                disabled
              />
              <v-text-field
                v-model="editedItem.car"
                label="Carro"
                :rules="[rules.required]"
                required
              />
              <v-text-field
                v-model="editedItem.car_model"
                label="Modelo"
                :rules="[rules.required]"
                required
              />
              <v-text-field
                v-model="editedItem.price"
                label="Precio"
                :rules="[rules.required]"
                required
              />
            </v-form>
          </v-card-text>
          <v-card-actions>
            <v-btn color="blue darken-1" text @click="editDialog = false">Cancelar</v-btn>
            <v-btn color="blue darken-1" text @click="saveItem">Guardar</v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
  
      <v-dialog v-model="deleteDialog" max-width="500px">
        <v-card>
          <v-card-title class="headline">Confirmar eliminación</v-card-title>
          <v-card-text>
            ¿Estás seguro de que quieres eliminar el auto <strong>{{ itemToDelete.car }}</strong>?
          </v-card-text>
          <v-card-actions>
            <v-btn color="red darken-1" text @click="deleteDialog = false">Cancelar</v-btn>
            <v-btn color="green darken-1" text @click="deleteItem">Eliminar</v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
  
      <v-alert v-if="error" type="error" dismissible>
        Hubo un error al cargar los datos. Por favor, inténtalo de nuevo.
      </v-alert>
    </v-container>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue';
  
  const autos = ref([]);
  const editedItem = ref({});
  const editDialog = ref(false);
  const deleteDialog = ref(false);
  const itemToDelete = ref(null);
  const valid = ref(false);
  const loading = ref(false);
  const error = ref(false);
  
  const headers = [
    { text: '#', value: 'id' },
    { text: 'Carro', value: 'car' },
    { text: 'Modelo', value: 'car_model' },
    { text: 'Precio', value: 'price' },
    { text: 'Acciones', value: 'actions', sortable: false },
  ];
  
  const rules = {
    required: value => !!value || 'Este campo es obligatorio',
  };
  
  const fetchAutos = async () => {
    loading.value = true;
    error.value = false;
  
    try {
      const response = await fetch('https://myfakeapi.com/api/cars');
      const data = await response.json();
      autos.value = data.cars.map((car, index) => ({
        id: index + 1,
        car: car.car,
        car_model: car.car_model,
        price: car.price,
      }));
    } catch (error) {
      console.error('Error al cargar los autos:', error);
      error.value = true;
    } finally {
      loading.value = false;
    }
  };
  
  onMounted(() => {
    fetchAutos();
  });
  
  const editItem = (item) => {
    editedItem.value = { ...item };
    editDialog.value = true;
  };
  
  const saveItem = () => {
    const index = autos.value.findIndex(auto => auto.id === editedItem.value.id);
    if (index !== -1) {
      autos.value.splice(index, 1);
      autos.value.push({ ...editedItem.value });
      autos.value.sort((a, b) => a.id - b.id);
    }
    editDialog.value = false;
  };
  
  const confirmDeleteItem = (item) => {
    itemToDelete.value = item;
    deleteDialog.value = true;
  };
  
  const deleteItem = () => {
    autos.value = autos.value.filter((auto) => auto.id !== itemToDelete.value.id);
    autos.value.forEach((auto, index) => {
      auto.id = index + 1;
    });
    autos.value.sort((a, b) => a.id - b.id);
    deleteDialog.value = false;
  };
  </script>
  
  <style scoped>
  .scrollable-table {
    max-height: 600px;
    overflow-y: auto;
  }
  </style>
  