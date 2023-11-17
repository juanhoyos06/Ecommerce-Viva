<template>
  <div>
    <v-sheet
      rounded="xl"
      elevation="12"
      max-width="400"
      width="90%"
      class="ma-10 pa-5 mx-auto"
    >
      <v-btn
        href="/"
        variant="text"
        class="position-absolute top-0 left-2"
        size="x-large"
        icon="mdi-arrow-left"
      ></v-btn>
      <center>
        <img
          src="../assets/img/logo_viva_login.png"
          height="200"
          class="ml-20"
        />
      </center>
      <v-sheet width="300" class="mx-auto">
        <br />
        <v-form fast-fail @submit.prevent="login">
          <v-text-field
            v-model="email"
            label="Correo"
            type="email"
            prepend-icon="mdi-email"
          ></v-text-field>
          <v-text-field
            v-model="password"
            :type="showPassword ? 'text' : 'password'"
            label="Contraseña"
            prepend-icon="mdi-key"
            :append-inner-icon="showPassword ? 'mdi-eye-off' : 'mdi-eye'"
            @click:append-inner="showPassword = !showPassword"
          ></v-text-field>
          <a href="#" class="text-body-2 font-weight-regular"
            >Olvidé mi contraseña</a
          >

          <v-btn type="submit" color="black" block class="mt-2">Ingresar</v-btn>
        </v-form>
        <div class="mt-2">
          <p class="text-body-2">
            ¿No tienes cuenta?
            <NuxtLink to="/register"> Registrarse </NuxtLink>
          </p>
        </div>
      </v-sheet>
    </v-sheet>
  </div>
</template>

<script setup>
import Swal from "sweetalert2";
import { currentUser } from "@/user.js";
import axios from "axios";
import config from '../config/default.json';


definePageMeta({layout: "blank",});

const email=  ref('')
const password=  ref('')
const showPassword=  ref(false)
const emailRules = ref([
  value => {
    if (value) return true
    return 'El campo es obligatorio.'
  },
  value => {
    if (/[^[^@]+@[^@]+\.[a-zA-Z]{2,}$/.test(value)) return true
    return 'Correo no valido.'
  }
])
const login = async () => {
  try {
    const url = `${config.api_host}/login`
    const {data} = await axios.post(url, {email: email.value, password: password.value})
    if (data?.ok){
      
      currentUser.setUser(data?.info)
      
      const id_rol = data?.info?.id_rol
      const token = data?.info?.token

      localStorage.setItem('token', token)
      if (id_rol === 3) {
        useRouter().push("/");
        
      } else if (id_rol === 1){
        useRouter().push("/shops");
        
      }
    }
    else{
      Swal.fire({
        title: 'Error!',
        text: data?.message,
        icon: 'error'
      })
    }
  } catch (error) {
    console.error(error);
    Swal.fire({
        title: 'Error!',
        text: 'Ha ocurrido un error al conectarse.',
        icon: 'error'
      })
  }
}


definePageMeta({
  layout: "ingreso",
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
