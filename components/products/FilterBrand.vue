<template>
    <div style="max-height: 20px00px; overflow-y: auto;">
      <v-list select-strategy="classic">
  
        <v-list-item v-for="brand in brands" :key="brand.id">
          <template v-slot:prepend="{ isActive }">
  
            <v-list-item-action start>
              <v-checkbox-btn :model-value="isActive"></v-checkbox-btn>
            </v-list-item-action>
          </template>
          <v-list-item-title>
            {{ brand.name }}
          </v-list-item-title>
  
        </v-list-item>
      </v-list>
    </div>
    <div style="text-align: right;">
      <v-btn style="background-color: #FFCC00;" variant="text" size="small">Aplicar</v-btn>
    </div>
  </template>
  <script setup>
  import axios from "axios";
  
  const brands = ref([])
  const selectedBrand = ref({})
  
  onBeforeMount(() => {
    loadTasks()
  })
  const loadTasks = async () => {
    const url = 'http://localhost:3001/brands'
    const { data } = await axios.get(url)
    console.log(data);
    brands.value = data
  }
  </script>