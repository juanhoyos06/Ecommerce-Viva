<template>
  <div>
    <v-app>
      <v-card color="grey-lighten-4" flat height="auto" rounded="0">
        <slot />
        <v-app-bar density="default">
          <img src="~/assets/img/logo_viva.png" width="200" height="200" class="mt-5">
          <v-spacer></v-spacer>
          <v-text-field class="ms-8" hide-details prepend-icon="mdi-magnify" variant="underlined"></v-text-field>
          <v-spacer></v-spacer>
          <v-btn class="ma-5" text="Compras" />
          <v-btn icon class="ma-5"><v-icon>mdi-bell</v-icon></v-btn>
          <v-btn icon class="ma-5"><v-icon>mdi-cart-minus</v-icon></v-btn>
          <v-btn prepend-icon="mdi-account-circle" stacked class="ma-5" @click="openUserDialog">Cuenta</v-btn> <!-- Corrected click handler -->

        </v-app-bar>
      </v-card>
    </v-app>

    <!-- Include the UserDialog component -->
    <UserDialog :user="user" ref="userDialog" />
  </div>
</template>

<script>
import UserDialog from "@/components/products/UserDialog.vue";
import { currentUser } from "@/user.js";

export default {
  data() {
    return {
      user: {
        name: "",
        email: "",
      },
    };
  },
  components: {
    UserDialog,
  },
  methods: {
    openUserDialog() {
      if(currentUser.user != undefined){
        this.$refs.userDialog.$data.dialog = true;
      }
      else{
        this.$router.push("/login");
      }
    },
  },
};
</script>
