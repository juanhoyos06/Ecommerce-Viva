<template>
  <v-dialog v-model="dialog" max-width="400">
    <v-card>
      <v-card-title>Información de usuario</v-card-title>
      <v-card-text>
        <div>
          <strong>Nombre:</strong>
          {{ currentUser.user ? currentUser.user.nombre : "" }}
        </div>
        <div>
          <strong>Correo:</strong>
          {{ currentUser.user ? currentUser.user.correo : "" }}
        </div>
      </v-card-text>
      <v-card-actions>
        <v-btn @click="editar" to="/editar-usuario">Editar</v-btn>
        <v-btn @click="logOut">Salir</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
import axios from "axios";
import config from '../../config/default.json';
import { currentUser } from "@/user.js";

export default {
  data() {
    return {
      dialog: false,
    };
  },
  methods: {
    async fetchUser() {
      try {
        const url = `${config.api_host}/user`
        const response = await axios.get(url, currentUser.id_usuario);
        console.log(response.data);
        
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
</script>
