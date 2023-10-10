<template>
  <div>
    <v-sheet
      rounded="xl"
      elevation="12"
      max-width="400"
      width="100%"
      class="ma-10 pa-5 mx-auto"
    >
      <v-btn
        href="/login"
        variant="text"
        class="position-absolute top-0 left-2"
        size="x-large"
        icon="mdi-arrow-left"
      ></v-btn>
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
        <v-form @submit.prevent="register">
          <v-text-field
            v-model="formData.name"
            label="Nombre"
            prepend-icon="mdi-account"
            required
          ></v-text-field>
          <v-text-field
            v-model="formData.email"
            label="Correo"
            prepend-icon="mdi-email"
            required
          ></v-text-field>
          <v-text-field
            v-model="formData.password"
            label="Contraseña"
            prepend-icon="mdi-key-variant"
            type="password"
            required
          ></v-text-field>
          <v-text-field
            v-model="formData.confirmPassword"
            label="Verificar Contraseña"
            prepend-icon="mdi-key-variant"
            type="password"
            required
          ></v-text-field>
          <v-checkbox
            v-model="formData.acceptTerms"
            label="Acepto términos y condiciones"
            required
          ></v-checkbox>
          <br />
          <v-btn type="submit" color="black" block class="mt-2">Guardar</v-btn>
        </v-form>
        <div class="mt-2">
          <p class="text-body-2">
            ¿Ya tienes cuenta? <NuxtLink to="/login">Inicia sesión</NuxtLink>
          </p>
        </div>
        <br />
      </v-sheet>
    </v-sheet>
  </div>
</template>

<script>
import axios from "axios";
import Swal from "sweetalert2";

export default {
  data() {
    return {
      formData: {
        name: "",
        email: "",
        password: "",
        confirmPassword: "",
        acceptTerms: false,
        visible: false,
        confirmVisible: false,
        users: [],
      },
    };
  },
  methods: {
    async getUsers() {
      try {
        const response = await axios.get("http://localhost:3001/users");
        this.formData.users = response.data; // Corrección: asignar a this.formData.users
      } catch (error) {
        console.error("Error al obtener usuarios:", error);
      }
    },
    async register() {
      await this.getUsers();

      // Check if the email already exists
      const emailExists = this.formData.users.some(
        (user) => user.email === this.formData.email // Corrección: usar this.formData.email
      );

      if (emailExists) {
        console.error("El correo electrónico ya está registrado.");
      } else if (this.formData.password !== this.formData.confirmPassword) {
        Swal.fire({
          title: "Error",
          text: "Las contraseñas no coinciden",
          icon: "error",
        });
      } else {
        // Increment the counter for the next user
        let highestId = this.formData.users.reduce((maxId, user) => {
          return user.id > maxId ? user.id : maxId;
        }, 0);

        highestId++;
        // Generate a unique sequential ID for the new user
        const userId = highestId;

        // Register the new user with the generated sequential ID
        const newUser = {
          id: userId,
          name: this.formData.name, // Corrección: usar this.formData.name
          email: this.formData.email, // Corrección: usar this.formData.email
          password: this.formData.password, // Corrección: usar this.formData.password
        };

        // Add the user to the server
        await this.addUser(newUser);

        Swal.fire({
          icon: "success",
          title: "Registro exitoso:",
        });

        console.log("Nuevo usuario registrado:", newUser);
        // Optionally, you can redirect to a different page after successful registration
        this.$router.push("./");
      }
    },
    async addUser(user) {
      try {
        const response = await axios.post("http://localhost:3001/users", user);
        console.log("Usuario agregado:", response.data);
      } catch (error) {
        console.error("Error al agregar usuario:", error);
      }
    },
  },
};
definePageMeta({
  layout: "blank",
});
</script>
