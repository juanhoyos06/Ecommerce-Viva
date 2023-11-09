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
            :type="showPassword ? 'text' : 'password'"
            label="Contraseña"
            prepend-icon="mdi-key-variant"
            :append-inner-icon="showPassword ? 'mdi-eye-off' : 'mdi-eye'"
            @click:append-inner="showPassword = !showPassword"
          ></v-text-field>
          <v-text-field
            v-model="formData.confirmPassword"
            :type="showPasswordC ? 'text' : 'password'"
            label="Verificar Contraseña"
            prepend-icon="mdi-key-variant"
            :append-inner-icon="showPasswordC ? 'mdi-eye-off' : 'mdi-eye'"
            @click:append-inner="showPasswordC = !showPasswordC"
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
      showPassword:false,
      showPasswordC:false,
    };
  },
  methods: {
    async getUsers() {
      try {
        const response = await axios.get("http://localhost:3001/users");
        this.formData.users = response.data;
      } catch (error) {
        console.error("Error al obtener usuarios:", error);
      }
    },
    async register() {
      if (
        !this.formData.name ||
        !this.formData.email ||
        !this.formData.password ||
        !this.formData.confirmPassword
      ) {
        Swal.fire({
          title: "Error",
          text: "Por favor, complete todos los campos del formulario",
          icon: "error",
        });
        return;
      }
      if (!this.formData.acceptTerms) {
        Swal.fire({
          title: "¡Lo siento!",
          text: "Debes aceptar terminos y condiciones para continuar",
          icon: "warning",
        });
        return;
      }

      // Validar el formato del correo electrónico
      const emailRegex = /^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,4}$/;
      if (!emailRegex.test(this.formData.email)) {
        Swal.fire({
          title: "Error",
          text: "El correo electrónico no tiene un formato válido",
          icon: "error",
        });
        return;
      }

      await this.getUsers();

      const emailExists = this.formData.users.some(
        (user) => user.email === this.formData.email
      );

      if (emailExists) {
        Swal.fire({
          title: "Lo siento",
          text: "El correo electrónico ya se encuentra registrado",
          icon: "warning",
        });
      } else if (this.formData.password !== this.formData.confirmPassword) {
        Swal.fire({
          title: "Error",
          text: "Las contraseñas no coinciden",
          icon: "error",
        });
      } else {
        let highestId = this.formData.users.reduce((maxId, user) => {
          return user.id > maxId ? user.id : maxId;
        }, 0);

        highestId++;
        const userId = highestId;

        const newUser = {
          id: userId,
          name: this.formData.name,
          email: this.formData.email,
          password: this.formData.password,
        };

        await this.addUser(newUser);

        Swal.fire({
          icon: "success",
          title: "Registro exitoso:",
        });

        this.$router.push("./login");
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
