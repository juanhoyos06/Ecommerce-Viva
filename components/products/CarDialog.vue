<template>
  <v-dialog v-model="dialog" max-width="700px" height="700px">
    <v-card style="border-radius: 20px">
      <v-toolbar color="#FFCC00" :elevation="8">
        <v-card-title class="card-title">CARRITO</v-card-title>
      </v-toolbar>
      <v-list :items="orders" item-props lines="three">
        <template v-slot:subtitle="{ subtitle }">
          <div v-html="subtitle"></div>
        </template>
      </v-list>


    </v-card>
  </v-dialog>
</template>

<script>
import axios from "axios";
import config from '../../config/default.json';
import { currentUser } from "@/user.js";

export default {
  data() {
    return {
      dialog: false,
      orders: []
    };
  },
  created() {
    if (this.dialog){

      this.getOrders();
    }
  },
  methods: {
    getHeaders() {
      const token = localStorage.getItem('token');
      return { headers: { 'Authorization': `Bearer ${token}` } }

    },
    async getUser() {

      const token = localStorage.getItem('token');


      if (token) {

        const url = `${config.api_host}/verify`;
        const user = await axios.post(url, { 'token': token })
        const _user = user?.data?.info;
        return _user;
      }
      return null
    },
    async getOrders() {
        
        const user = await this.getUser();
        
        const id = user.id_usuario;

        const urlS = `${config.api_host}/users/productsCar/${id}`;
        const headers = this.getHeaders();
        const { data } = await axios.get(urlS, { headers });
        
        this.orders = data?.info;


      },
      editar() {
        this.dialog = false;
      },
      logOut() {
        // Implement your logout logic here
        currentUser.setUser(null);
        localStorage.removeItem('token')
        this.$router.push("/login");
      },
    },
    computed: {
      currentUser() {
        return currentUser;
      },
    },
    mounted() {
      if (!currentUser.user) {
        // this.fetchUser();
      }
    },
  };
</script>
