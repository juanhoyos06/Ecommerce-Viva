<template>
    <v-item-group selected-class="bg-primary">
        <v-container fluid>
            <v-row align="center">
                <v-col cols="9">
                    <v-text-field :loading="loading" density="compact" variant="solo" label="Buscar categoria"
                        append-inner-icon="mdi-magnify" single-line hide-details @click:append-inner="onClick"
                        style="width:50%;" class="mx-auto text-center"></v-text-field>
                </v-col>
                <ShopsNewCategoryDialog />

            </v-row>

            <br>
            <br>
            
            <v-row>
                <v-container fluid>
                    <v-table weidth="100vh">
                        <thead>
                            <tr>
                                <th class="text-left">
                                    Categoria
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
                            <tr v-for="item in categories" :key="item.id_categoria">
                                <td>{{ item.nombre }}</td>
                                <td>{{ item.count }}</td>
                                <td>
                                    <ShopsEditCategoryDialog :selectedCategory="item.id_categoria" />
                                    <v-btn icon="mdi-delete-off" variant="text" @click="deleteCategory(item.id_categoria)">
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
import config from '../../config/default.json';
import Swal from "sweetalert2";

export default {
    data() {
        return {
            loaded: false,
            loading: false,
            categories: [],
            
        };
    },
    created() {
        this.loadCategories();
    },
    methods: {
        onClick() {
            this.loading = true

            setTimeout(() => {
                this.loading = false
                this.loaded = true
            }, 2000)
        },
        getHeaders() {
            const token = localStorage.getItem('token');
            return { headers: { 'Authorization': `Bearer ${token}` } }

        },
        async loadCategories() {
            const url = `${config.api_host}/categories/shops`;
            const headers = this.getHeaders();
            const { data } = await axios.get(url, { headers });
            this.categories = data.info;

        },
        deleteCategory(id){
            Swal.fire({
                title: '¿Estas seguro?',
                text: "Una vez eliminado no podra reversar la operación",
                icon: 'warning',
                showCancelButton: true,
                confirmButtonColor: '#3085d6',
                cancelButtonColor: '#d33',
                confirmButtonText: 'Si, eliminar!'
            }).then(async (result) => {
                if (result.isConfirmed) {
                    const url = `${config.api_host}/categories/${id}`;
                    const { data } = await axios.delete(url);
                    this.loadCategories();
                    Swal.fire(
                        'Eliminado!',
                        'La categoria se eliminó correctamente',
                        'success'
                    )
                }
            })
        }
    }
}
</script>