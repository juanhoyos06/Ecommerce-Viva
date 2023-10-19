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
            <v-card-title class="headline">Editar perfil</v-card-title>
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
import Swal from "sweetalert2";

export default {
  data() {
    return {
      dialog: false,
      id: currentUser.user ? currentUser.user.id : "",
      name: currentUser.user ? currentUser.user.name : "",
      email: currentUser.user ? currentUser.user.email : "",
      password: currentUser.user ? currentUser.user.password : "",
      emailValid: true, // Agregar propiedad para la validación del correo
    };
  },
  methods: {
    async guardarCambios() {
      // Validar el correo electrónico
      this.emailValid = this.validateEmail(this.email);

      if (this.emailValid) {
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
          Swal.fire({
            title: "Listo",
            text: "Perfil actualizado",
            icon: "success",
          });
          this.$router.push("./");
        } catch (error) {
          console.error("Error al actualizar el usuario", error);
          // Manejar errores o mostrar un mensaje de error al usuario
        }
      } else {
        // Mostrar una alerta si el correo no es válido
        Swal.fire({
          title: "Error",
          text: "El correo electrónico no tiene un formato válido",
          icon: "error",
        });
      }
    },
    validateEmail(email) {
      // Expresión regular para validar el formato de un correo electrónico
      const emailRegex = /^[a-zA-Z0-9._-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,4}$/;
      return emailRegex.test(email);
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
