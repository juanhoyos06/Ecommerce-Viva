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

                  <v-list-item v-for="category in categories" :key="category.id">
                    <template v-slot:prepend="{ isActive }">

                      <v-list-item-action start>
                        <v-checkbox-btn :value="category.name" v-model="selectedCategories"></v-checkbox-btn>
                      </v-list-item-action>
                    </template>
                    <v-list-item-title>
                      {{ category.name }}
                    </v-list-item-title>

                  </v-list-item>
                </v-list>
              </div>

              <v-list-item-title>Marcas</v-list-item-title>
              <div style="max-height: 20px00px; overflow-y: auto;">
                <v-list select-strategy="classic">

                  <v-list-item v-for="brand in brands" :key="brand.id">
                    <template v-slot:prepend="{ isActive }">

                      <v-list-item-action start>
                        <v-checkbox-btn :value="brand.name" v-model="selectedBrands"></v-checkbox-btn>
                      </v-list-item-action>
                    </template>
                    <v-list-item-title>
                      {{ brand.name }}
                    </v-list-item-title>

                  </v-list-item>
                </v-list>
              </div>
              <v-list-item-title>Tiendas</v-list-item-title>
              <div style="max-height: 20px00px; overflow-y: auto;">
                <v-list select-strategy="classic">

                  <v-list-item v-for="shop in shops" :key="shop.id">
                    <template v-slot:prepend="{ isActive }">

                      <v-list-item-action start>
                        <v-checkbox-btn :value="shop.name" v-model="selectedShop"></v-checkbox-btn>
                      </v-list-item-action>
                    </template>
                    <v-list-item-title>
                      {{ shop.name }}
                    </v-list-item-title>

                  </v-list-item>
                </v-list>
              </div>

              <v-list-item-title>Precios</v-list-item-title>

            </v-list>
          </v-navigation-drawer>
        </v-col>
        <v-col cols="9" class="pa-0">
          <v-main class="pa-0 overflow-y-auto" style="max-height: 100vh">
            <ProductsViewProducts v-if="selectedCategories.length > 0" :filter="selectedCategories" />
            <ProductsViewProducts v-else />

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

export default {
  name: 'FilterCategory',

  data() {
    return {
      categories: [],
      selectedCategories: [],
      brands: [],
      selectedBrands: [],
      shops: [],
      selectedShop: []
    };
  },
  created() {
    this.loadCategories();
    this.loadBrands();
    this.loadShops();
  },
  methods: {
    async loadCategories() {
      const url = 'http://localhost:3001/categories';
      const { data } = await axios.get(url);
      this.categories = data;
    },
    async loadBrands() {
      const url = 'http://localhost:3001/brands';
      const { data } = await axios.get(url);
      this.brands = data;
    },
    async loadShops() {
      const url = 'http://localhost:3001/shops';
      const { data } = await axios.get(url);
      this.shops = data;
    }
  }
}

</script>
