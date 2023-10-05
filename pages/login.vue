<template>
  <div>
    <v-sheet rounded="xl" elevation="12" max-width="400" width="90%" class="ma-10 pa-5 mx-auto">
      <v-btn href="/" variant="text" class="position-absolute top-0 left-2" size="x-large"
        icon="mdi-arrow-left"></v-btn>
      <center>
        <img src="../assets/img/logo_viva_login.png" height="200" class="ml-20">
      </center>
      <v-sheet width="300" class="mx-auto">
        <br>
        <v-form fast-fail @submit.prevent="login">
          <v-text-field v-model="email" label="Correo" type="email" prepend-icon="mdi-email"></v-text-field>
          <v-text-field v-model="password" type="password" label="Contraseña" prepend-icon="mdi-key"></v-text-field>
          <a href="#" class="text-body-2 font-weight-regular">Olvide mi contraseña</a>

          <v-btn type="submit" color="black" block class="mt-2">Ingresar</v-btn>

        </v-form>
        <div class="mt-2">
          <p class="text-body-2">¿No tiene cuenta?
            <NuxtLink to="/register">
              Registrarse
            </NuxtLink>
          </p>
        </div>
      </v-sheet>
    </v-sheet>
  </div>
</template>
<script>
import axios from 'axios';

export default {
  data() {
    return {
      email: '',
      password: '',
      visible: false,
      users: [],
      emailRules: [
        value => {
          if (value) return true
          return 'El campo es obligatorio.'
        },
        value => {
          if (/[^[^@]+@[^@]+\.[a-zA-Z]{2,}$/.test(value)) return true
          return 'Correo no valido.'
        }
      ],
    };
  },
  methods: {
    async getUsers() {
      try {
        const response = await axios.get('http://localhost:3001/users');
        this.users = response.data;
      } catch (error) {
        console.error('Error al obtener usuarios:', error);
      }
    },
    async login() {
      await this.getUsers();

      const foundUser = this.users.find(
        user =>
          user.email === this.email && user.password === this.password
      );

      if (foundUser) {
        console.log('Inicio de sesión exitoso para el usuario:', foundUser);
        this.$router.push('/');
      } else {
        console.error('Credenciales incorrectas. Inicio de sesión fallido.');
      }
    },
  },
};
definePageMeta({
  layout: "blank",
});
</script>
<style>
body {
  width: 100%;
  height: 100%;
  background-image: url(./assets/img/fondo_login.jpg);
  background-size: cover;

}
</style>
