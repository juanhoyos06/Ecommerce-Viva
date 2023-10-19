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
              <v-text-field v-model="name" label="Nombre"></v-text-field>
              <v-text-field v-model="email" label="Correo"></v-text-field>
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
      id: currentUser.user ? currentUser.user.id : "",
      name: currentUser.user ? currentUser.user.name : "",
      email: currentUser.user ? currentUser.user.email : "",
      password: currentUser.user ? currentUser.user.password : "",
    };
  },
  methods: {
    async guardarCambios() {
      try {
        // Preparar los datos para actualizar
        const dataToUpdate = {
          id: this.id,
          name: this.name,
          email: this.email,
          password: this.password,
        };

        // Realizar la solicitud PUT a la base de datos (ajusta la URL según tu API)
        await axios.put(
          `http://localhost:3001/users/${currentUser.user.id}`,
          dataToUpdate
        );

        // Actualizar el usuario en currentUser si es necesario
        currentUser.setUser({
          ...currentUser.user,
          name: this.name,
          email: this.email,
        });

        // Redirigir o mostrar un mensaje de éxito
        this.$router.push("./");
      } catch (error) {
        console.error("Error al actualizar el usuario", error);
        // Manejar errores o mostrar un mensaje de error al usuario
      }
    },
    logOut() {
      currentUser.setUser(null);
      this.$router.push("/login");
    },
  },
  computed: {
    currentUser() {
      return currentUser;
    },
  },
  definePageMeta: {
    layout: "blank",
  },
};
</script>
