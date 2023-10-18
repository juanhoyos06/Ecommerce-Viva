<template>
  <div style="max-height: 20px00px; overflow-y: auto;">
    <v-list select-strategy="classic">

      <v-list-item v-for="category in categories" :key="category.id">
        <template v-slot:prepend="{ isActive }">

          <v-list-item-action start>
            <v-checkbox-btn :value="category.id" v-model="selectedCategories"></v-checkbox-btn>
          </v-list-item-action>
        </template>
        <v-list-item-title>
          {{ category.name }}
        </v-list-item-title>

      </v-list-item>
    </v-list>
  </div>
  <div style="text-align: right;">
    <v-btn style="background-color: #FFCC00;" variant="text" size="small" @click="loadFilteredProducts">Aplicar</v-btn>
  </div>
</template>
<script setup>
import axios from "axios";

const categories = ref([])
const selectedCategories = ref([])

onBeforeMount(() => {
  loadCategories()
})
const loadCategories = async () => {
  const url = 'http://localhost:3001/categories'
  const { data } = await axios.get(url)
  console.log(data);
  categories.value = data
}
const loadFilteredProducts = () => {
  selectedCategories.value.forEach(categoryId => {
    const category = categories.value.find(cat => cat.id === categoryId);
    if (category) {
      console.log(category.name);
    }
  });
}

</script>