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
              <ProductsFilterCategory />

              <v-list-item-title>Marcas</v-list-item-title>
              <ProductsFilterBrand />

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
</script>