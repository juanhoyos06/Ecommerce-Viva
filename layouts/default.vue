<template>
  <div>
    <v-app>
      <v-card color="grey-lighten-4" flat height="auto" rounded="0">
        <slot />
        <v-app-bar density="default">

          <img src="~/assets/img/logo_viva.png" width="200" height="200" class="mt-5">

          <v-spacer> </v-spacer>


          <v-text-field class="ms-8" hide-details prepend-icon="mdi-magnify" variant="underlined"></v-text-field>
          <v-spacer> </v-spacer>
          <v-btn class="ma-5" text="Compras" />


          <v-btn icon class="ma-5">
            <v-icon>mdi-bell</v-icon>
          </v-btn>

          <v-btn icon class="ma-5">
            <v-icon>mdi-cart-minus</v-icon>
          </v-btn>

          <v-btn class="ma-5" prepend-icon="mdi-account-circle" stacked href="/login">
            Cuenta
          </v-btn>


        </v-app-bar>
      </v-card>
    </v-app>


  </div>
</template>
<style>
body {
  background-image: none;
}
</style>