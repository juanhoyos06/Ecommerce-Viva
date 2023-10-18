<template>
  <v-dialog v-model="dialog" max-width="400">
    <v-card>
      <v-card-title>User Information</v-card-title>
      <v-card-text>
        <div>
          <strong>Name:</strong> {{ user.name }}
        </div>
        <div>
          <strong>Email:</strong> {{ user.email }}
        </div>
      </v-card-text>
      <v-card-actions>
        <v-btn @click="goToProfile">Go to Profile</v-btn>
        <v-btn @click="logOut">Log Out</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      dialog: false,
      user: {
        name: "",
        email: "",
      },
    };
  },
  methods: {
    async fetchUser() {
      try {
        const response = await axios.get("http://localhost:3001/users"); // Fetch the first user, or adjust the URL as needed
        this.user = response.data[0]; // Assuming the first user in the response is the one you want
      } catch (error) {
        console.error("Error fetching user data", error);
      }
    },
    goToProfile() {
      this.$router.push("/profile");
    },
    logOut() {
      // Implement your logout logic here
      this.$router.push("/login");
    },
  },
  mounted() {
    // Fetch user data when the component is mounted
    this.fetchUser();
  },
};
</script>
