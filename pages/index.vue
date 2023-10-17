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
              <v-list-item prepend-icon="mdi-email" title="Inbox" value="inbox"></v-list-item>
              <v-list-item prepend-icon="mdi-account-supervisor-circle" title="Supervisors"
                value="supervisors"></v-list-item>
              <v-list-item-title>Marcas</v-list-item-title>
              <v-list-item prepend-icon="mdi-email" title="Inbox" value="inbox"></v-list-item>
              <v-list-item prepend-icon="mdi-account-supervisor-circle" title="Supervisors"
                value="supervisors"></v-list-item>
              <v-list-item-title>Precios</v-list-item-title>
              <v-list-item prepend-icon="mdi-email" title="Inbox" value="inbox"></v-list-item>
              <v-list-item prepend-icon="mdi-account-supervisor-circle" title="Supervisors"
                value="supervisors"></v-list-item>
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
export default {
  data() {
    return {
      dialog: false,
      characters: [],
      selectedCharacter: {}

    }
  },
  mounted() {
    axios.get('https://gateway.marvel.com:443/v1/public/characters?ts=1&apikey=699987288fb2dc8a3483835fc5d27322&hash=3df1e8d77029aa53d48fc11664f3a491&limit=100')
      .then(response => {
        this.characters = response.data.data.results.filter(hero => {
          return hero.thumbnail.path != 'http://i.annihil.us/u/prod/marvel/i/mg/b/40/image_not_available';
        });
      })
      .catch(error => {
        console.error("Hubo un error", error);
      });
  },
  methods: {
    openDialog(item) {
      this.selectedCharacter = item;  // Guarda el personaje seleccionado
      this.dialog = true;  // Abre el v-dialog
    }
  }
};

</script>