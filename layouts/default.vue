<template>
  <div>
    <v-app>
      <v-card color="grey-lighten-4" flat height="auto" rounded="0">
        <slot />
        <v-app-bar density="default">
          <img src="~/assets/img/logo_viva.png" width="200" height="200" class="mt-5">
          <v-spacer></v-spacer>
          <!-- <v-spacer></v-spacer> -->
          <v-btn icon class="ma-5" @click="openCarDialog">

            <v-badge color="error" :content="products">
              <v-icon>
                mdi-cart
              </v-icon>
            </v-badge>
          </v-btn>
          <v-btn prepend-icon="mdi-account-circle" stacked class="ma-5" @click="openUserDialog">{{user.name}}</v-btn>
          <!-- Corrected click handler -->

        </v-app-bar>
      </v-card>
    </v-app>

    <!-- Include the UserDialog component -->
    <UserDialog :user="user" ref="userDialog" />
    <CarDialog :user="user" ref="carDialog" />
  </div>
</template>

<script>
import UserDialog from "@/components/products/UserDialog.vue";
import CarDialog from "@/components/products/CarDialog.vue";
import axios from "axios";
import config from '../config/default.json';

export default {
  data() {
    return {
      user: {
        name: "Cuenta",
        email: "",
      },
      products: '',


    };
  },
  created() {
    this.intervalo = setInterval(this.getNumberCar, 5000);
  },
  components: {
    UserDialog,
    CarDialog
  },
  methods: {
    async openUserDialog() {
      const id = await this.getIdUser();


      if (id?.id_usuario) {
        this.$refs.userDialog.$data.dialog = true;

      }
      else {
        this.$router.push("/login");
      }
    },
    async openCarDialog() {
      const id = await this.getIdUser();


      if (id?.id_usuario) {
        
        this.$refs.carDialog.$data.dialog = true;

      }
      else {
        this.$router.push("/login");
      }
    },
    getHeaders() {
      const token = localStorage.getItem('token');
      return { headers: { 'Authorization': `Bearer ${token}` } }

    },
    async getIdUser() {

      const token = localStorage.getItem('token');


      if (token) {

        const url = `${config.api_host}/verify`;
        const user = await axios.post(url, { 'token': token })
        const _user = user?.data?.info;
        return _user;
      }
      return null

    },
    async getNumberCar() {
      const user = await this.getIdUser();
      const id = user.id_usuario

      if (id) {

        const url = `${config.api_host}/users/infoCar/${id}`;
        const headers = this.getHeaders();
        const { data } = await axios.get(url, { headers });
        this.products = data?.info?.count;

      }
    }
  }
}
</script>
