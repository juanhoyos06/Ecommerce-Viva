<template>
  <v-dialog v-model="dialog" max-width="400">
    <v-card>
      <v-card-title>Información de usuario</v-card-title>
      <v-card-text>
        <div>
          <strong>Nombre:</strong>
          {{ name }}
        </div>
        <div>
          <strong>Correo:</strong>
          {{ email }}
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
      name: '',
      emai: ''
    };
  },
  created(){
    this.fetchUser();
  },
  methods: {
    async fetchUser() {
      
        const token = localStorage.getItem('token');
        

        
        const url = `${config.api_host}/verify`;
        const user = await axios.post(url, { 'token': token })
        const _user = user?.data?.info;

        

        this.name = _user.nombre
        this.email = _user.correo

     
    },
    editar() {
      this.dialog = false;
    },
    logOut() {
      // Implement your logout logic here
      currentUser.setUser(null);
      localStorage.removeItem('token')
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
