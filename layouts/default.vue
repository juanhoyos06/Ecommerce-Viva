<template>
  <div>
    <v-app>
      <v-card color="grey-lighten-4" flat height="auto" rounded="0">
        <slot />
        <v-app-bar density="default">
          <img src="~/assets/img/logo_viva.png" width="200" height="200" class="mt-5">
          <v-spacer></v-spacer>
          <!-- <v-spacer></v-spacer> -->
          <v-btn class="ma-5" text="Compras" />
          <v-btn icon class="ma-5">
            <v-badge dot color="success">
              <v-icon>
                mdi-bell

              </v-icon>
            </v-badge>
          </v-btn>
          <v-btn icon class="ma-5">
            
            <v-badge color="error" :content="products">
            <v-icon>
              mdi-cart
            </v-icon>
          </v-badge>
          </v-btn>
          <v-btn prepend-icon="mdi-account-circle" stacked class="ma-5" @click="openUserDialog">Cuenta</v-btn>
          <!-- Corrected click handler -->

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
      products: 3,

    };
  },
  components: {
    UserDialog,
  },
  methods: {
    openUserDialog() {
      if (currentUser.user != undefined) {
        this.$refs.userDialog.$data.dialog = true;
      }
      else {
        this.$router.push("/login");
      }
    },
  },
};
</script>
