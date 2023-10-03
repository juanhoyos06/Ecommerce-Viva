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
<script setup>
definePageMeta({
  layout: "blank",
});

const email = ref("");
const password = ref("");

</script>
<style>
body {
  width: 100%;
  height: 100%;
  background-image: url(./assets/img/fondo_login.jpg);
  background-size: cover;

}
</style>
