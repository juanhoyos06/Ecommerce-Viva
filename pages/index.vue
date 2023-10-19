<template>
  <v-card class="overflow-hidden">
    <v-layout>
      <v-row class="ma-0">
        <v-col cols="3" class="pa-0">
          <v-navigation-drawer permanent theme="default">
            <v-list nav>
              <br><br><br>
              <v-card-title class="card_title">Filtros</v-card-title>
              <br>
              <v-list-item-title>Categorias</v-list-item-title>
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
                <v-btn style="background-color: #FFCC00;" variant="text" size="small"
                  @click="loadSelectedCategories">Aplicar</v-btn>
              </div>
              <v-list-item-title>Marcas</v-list-item-title>
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
                <v-btn style="background-color: #FFCC00;" variant="text" size="small" @click="loadSelectedBrands">Aplicar</v-btn>
              </div>

              <v-list-item-title>Precios</v-list-item-title>


              <v-list-item prepend-icon="mdi-clock-start" title="Clock-in" value="clockin"></v-list-item>
            </v-list>
          </v-navigation-drawer>
        </v-col>
        <v-col cols="9" class="pa-0">
          <v-main class="pa-0 overflow-y-auto" style="max-height: 100vh;">
            <ProductsViewProducts />
          </v-main>
        </v-col>
      </v-row>
    </v-layout>
  </v-card>
</template>
<script>


definePageMeta({
  layout: "default",
});

import axios from "axios";
import { componentMethodsMixin } from '~/components/products/ViewProducts.vue'

export default {
  name: 'FilterCategory', // Puedes poner el nombre que desees para tu componente
  mixins: [componentMethodsMixin],
  data() {
    return {
      categories: [],
      selectedCategories: [],
      brands: [],
      selectedBrands: []
    };
  },
  created() {
    this.loadCategories();
    this.loadBrands();
  },
  methods: {
    async loadCategories() {
      const url = 'http://localhost:3001/categories';
      const { data } = await axios.get(url);
      console.log(data);
      this.categories = data;
    },
    async loadBrands() {
      const url = 'http://localhost:3001/brands';
      const { data } = await axios.get(url);
      console.log(data);
      this.brands = data;
    },
    loadSelectedCategories() {
      this.selectedCategories.forEach(categoryId => {
        const category = this.categories.find(cat => cat.id === categoryId);
        if (category) {
          console.log(category.name);
        this.myExportedMethod(category.name);

        }
      });
    },
    loadSelectedBrands() {
      this.selectedCategories.forEach(brandId => {
        const brand = this.brands.find(bra => bra.id === brandId);
        if (brand) {
          console.log(brand.name);
        }
      });
    }
  }
}
</script>