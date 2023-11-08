<template>
    <v-item-group selected-class="bg-primary">
        <v-container fluid>
            <v-row align="center">
                <v-col cols="9">
                    <v-text-field :loading="loading" density="compact" variant="solo" label="Buscar marca"
                        append-inner-icon="mdi-magnify" single-line hide-details @click:append-inner="onClick"
                        style="width:50%;" class="mx-auto text-center"></v-text-field>
                </v-col>
                <ShopsNewBrandDialog />

            </v-row>

            <br>
            <br>
            
            <v-row>
                <v-container fluid>
                    <v-table weidth="100vh">
                        <thead>
                            <tr>
                                <th class="text-left">
                                    Marca
                                </th>
                                <th class="text-left">
                                    Cantidad productos
                                </th>
                                <th class="text-left">
                                    Acciones
                                </th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="brand in brands" :key="brand.id">
                                <td>{{ brand.name }}</td>
                                <td>{{ brand.description }}</td>
                                <td>
                                    <v-btn icon="mdi-pencil" variant="text" @click="editTasks(brand)"></v-btn>
                                    <v-btn icon="mdi-delete-off" variant="text" @click="deleteTasks(brand)">
                                    </v-btn>
                                </td>
                            </tr>
                        </tbody>
                    </v-table>
                </v-container>
            </v-row>
        </v-container>
    </v-item-group>
</template>
<script>
import axios from "axios";

export default {
    data() {
        return {
            loaded: false,
            loading: false,
            brands: [],
            
        };
    },
    created() {
        this.loadBrands();
    },
    methods: {
        onClick() {
            this.loading = true

            setTimeout(() => {
                this.loading = false
                this.loaded = true
            }, 2000)
        },
        async loadBrands() {
            const url = 'http://localhost:3001/brands';
            const { data } = await axios.get(url);
            this.brands = data;
        }
    }
}
</script>