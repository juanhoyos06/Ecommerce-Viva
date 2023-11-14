<template>
  <v-card class="overflow-hidden">
    <v-layout>
      <v-row class="ma-0">
        <v-col cols="3" class="pa-0">
          <v-navigation-drawer permanent theme="default">
            <v-list nav>
              <br /><br /><br />
              <v-card-title class="card_title">Filtros</v-card-title>
              <br />
              <v-list-item-title>Categorias</v-list-item-title>
              <div style="max-height: 20px00px; overflow-y: auto;">

                <v-list select-strategy="classic">

                  <v-list-item v-for="category in categories" :key="category.id_categoria">
                    <template v-slot:prepend="{ isActive }">

                      <v-list-item-action start>
                        <v-checkbox-btn :value="category.id_categoria" v-model="selectedCategory"></v-checkbox-btn>
                      </v-list-item-action>
                    </template>
                    <v-list-item-title>
                      {{ category.nombre }}
                    </v-list-item-title>

                  </v-list-item>
                </v-list>
              </div>

              <v-list-item-title>Marcas</v-list-item-title>
              <div style="max-height: 20px00px; overflow-y: auto;">
                <v-list select-strategy="classic">

                  <v-list-item v-for="brand in brands" :key="brand.id_marca">
                    <template v-slot:prepend="{ isActive }">

                      <v-list-item-action start>
                        <v-checkbox-btn :value="brand.id_marca" v-model="selectedBrand"></v-checkbox-btn>
                      </v-list-item-action>
                    </template>
                    <v-list-item-title>
                      {{ brand.nombre }}
                    </v-list-item-title>

                  </v-list-item>
                </v-list>
              </div>
              

            </v-list>
          </v-navigation-drawer>
        </v-col>
        <v-col cols="9" class="pa-0">
          <v-main class="pa-0 overflow-y-auto" style="height: 100vh">
            <ProductsViewProducts v-if="selectedCategory && !selectedBrand" :filterC="selectedCategory" />
            <ProductsViewProducts v-if="!selectedCategory && selectedBrand" :filterB="selectedBrand" />
            <ProductsViewProducts v-if="(!selectedCategory && !selectedBrand) ||(selectedCategory && selectedBrand) " />
            

          </v-main>
        </v-col>
      </v-row>
    </v-layout>
  </v-card>
</template>
<script>

import axios from "axios";
import config from '../config/default.json';




export default {
  nombre: 'Filters',

  data() {
    return {
      categories: [],
      selectedCategory: null,
      brands: [],
      selectedBrand: null
      
    };
  },
  created() {
    this.loadCategories();
    this.loadBrands();
    
  },
  methods: {
    getHeaders() {
      const token = localStorage.getItem('item');
      return { headers: { 'Authorization': `Bearer ${token}` } }

    },
    async loadCategories() {
      const url = `${config.api_host}/categories`;
      const headers = this.getHeaders();
      const { data } = await axios.get(url, {headers});
      this.categories = data.info;
    },
    async loadBrands() {
      const url =  `${config.api_host}/brands`;
      const headers = this.getHeaders();
      const { data } = await axios.get(url, {headers});
      this.brands = data.info;
    }
  }
}

</script>
