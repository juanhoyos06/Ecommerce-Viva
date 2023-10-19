<template>
  <div>
    <v-sheet
      rounded="xl"
      elevation="12"
      max-width="400"
      width="100%"
      class="ma-10 pa-5 mx-auto"
    >
      <div class="text-center">
        <img
          src="~/assets/img/logo_viva_login.png"
          width="200"
          height="200"
          class="ml-20"
        />
      </div>
      <br />
      <v-sheet width="300" class="mx-auto">
        <div class="mt-2">
          <v-card>
            <v-card-title class="headline">Editar Perfil</v-card-title>
            <v-card-text>
              <v-text-field
                v-model="name"
                label=""
                :value="currentUser.user ? currentUser.user.name : 'nombre: '"
              ></v-text-field>

              <v-text-field
                v-model="email"
                label=""
                :value="currentUser.user ? currentUser.user.email : ''"
              ></v-text-field>
            </v-card-text>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="primary" to="./">Cancelar</v-btn>
              <v-btn color="primary" @click="guardarCambios">Guardar</v-btn>
            </v-card-actions>
          </v-card>
        </div>
        <br />
      </v-sheet>
    </v-sheet>
  </div>
</template>

<script>
import axios from "axios";
import { currentUser } from "@/user.js";

export default {
  data() {
    return {
      dialog: false,
      name: "", // Agregar las variables para name y email
      email: "",
    };
  },
  methods: {
    async fetchUser() {
      try {
        const response = await axios.get("http://localhost:3001/users");
        //currentUser.setUser(response.data[0]);
      } catch (error) {
        console.error("Error fetching user data", error);
      }
    },
    editar() {
      this.dialog = false;
    },
    logOut() {
      // Implement your logout logic here
      currentUser.setUser(null);
      this.$router.push("/login");
    },
  },
  computed: {
    currentUser() {
      return currentUser;
    },
  },
  mounted() {
    if (!currentUser.user) {
      this.fetchUser();
    }
  },
};
definePageMeta({
  layout: "blank",
});
</script>
