<template>
  <v-card>
    <v-layout>
      <v-navigation-drawer permanent theme="default">
          <v-list nav>
            <br><br><br>
            <v-card-title class="card_title">Filtros</v-card-title>
            <br>
            <v-list-item-title>Categorias</v-list-item-title>
            <v-list-item prepend-icon="mdi-email" title="Inbox" value="inbox"></v-list-item>
            <v-list-item prepend-icon="mdi-account-supervisor-circle" title="Supervisors"
            value="supervisors"></v-list-item>
            <v-list-item-title>Marcas</v-list-item-title>
            <v-list-item prepend-icon="mdi-email" title="Inbox" value="inbox"></v-list-item>
            <v-list-item prepend-icon="mdi-account-supervisor-circle" title="Supervisors"
            value="supervisors"></v-list-item>
            <v-list-item-title>Precios</v-list-item-title>
            <v-list-item prepend-icon="mdi-email" title="Inbox" value="inbox"></v-list-item>
            <v-list-item prepend-icon="mdi-account-supervisor-circle" title="Supervisors"
              value="supervisors"></v-list-item>
            <v-list-item prepend-icon="mdi-clock-start" title="Clock-in" value="clockin"></v-list-item>
          </v-list>
        </v-navigation-drawer>
      <v-main style="height:700px"></v-main>
    </v-layout>
  </v-card>
</template>
<script>
definePageMeta({
  layout: "default",
});
</script>