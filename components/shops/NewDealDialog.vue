<template>
    <v-col cols="1">
        <v-tooltip text="Agregar promocion">
            <template v-slot:activator="{ props }">
                <v-btn icon="mdi-plus" color="#FFCC00" class="mr-4" v-bind="props" @click="dialog = true"></v-btn>
            </template>
        </v-tooltip>
    </v-col>
    <v-dialog v-model="dialog" width="60%">
        <v-card>
            <v-toolbar color="#FFCC00" :elevation="8">
                <v-card-title class="card-title">NUEVA PROMOCIÓN</v-card-title>

            </v-toolbar>

            <v-card-text>
                <form action="javascript:void(0)" @submit="saveDeal()">
                    <v-row>
                        <v-col cols="6">

                            <v-text-field label="Descripcion" v-model="description" required variant="underlined"
                                placeholder="Ingrese la descripción de la promocion" style="width:100%;" />


                            <v-text-field label="Fecha inicio" v-model="start_date" variant="underlined" style="width:100%;"
                                type="date" />

                            <v-text-field label="Fecha finalización" v-model="end_date" variant="underlined"
                                style="width:100%;" type="date" />

                            <v-text-field label="Porcentaje" v-model="percentage" required variant="underlined"
                                placeholder="Ingrese el porcentaje de descuento" style="width:100%;" type="number" />


                        </v-col>

                        <v-col cols="6">
                            <v-combobox label="Categoria" :items="categories" v-model="category" variant="underlined"
                                style="width:100%;"></v-combobox>

                            <v-file-input label="Imagen" variant="underlined" v-model="img" style="width:100%;"
                                accept=".jpg, .jpeg, .png"></v-file-input>

                        </v-col>


                    </v-row>

                    <v-row justify="end" class="mt-4">
                        <v-col cols="2">
                            <v-btn @click="dialog = false">Cancelar</v-btn>
                        </v-col>
                        <v-col cols="2">
                            <v-btn color="#FFCC00" type="submit">Guardar</v-btn>
                        </v-col>
                    </v-row>
                    <br>

                </form>

            </v-card-text>

        </v-card>
    </v-dialog>
</template>
<script>
import axios from "axios";
import config from '../../config/default.json';
import Swal from "sweetalert2";

export default {
    data() {
        return {
            dialog: false,
            deal: {},
            categories: [],
            brands: [],
            start_date: '',
            end_date: '',
            category: '',
            description: '',
            percentage: '',
            category: '',
            img: ''

        };

    },
    created() {
        this.loadCategories();
    },
    methods: {
        getHeaders() {
            const token = localStorage.getItem('token');
            return { headers: { 'Authorization': `Bearer ${token}` } }

        },
        async getIdUser() {

            const token = localStorage.getItem('token');
            const url = `${config.api_host}/verify`;
            const user = await axios.post(url, { 'token': token })
            const id = user?.data?.info?.id_usuario;
            return id;

        },
        async saveDeal() {
            try {
                const url = `${config.api_host}/deals`;
                const formData = new FormData();
                formData.append('description', this.description);
                formData.append('id_category', this.category);
                formData.append('startDate', this.start_date);
                formData.append('endDate', this.end_date);
                formData.append('id_user', await this.getIdUser());
                formData.append('percentage', this.percentage);
                formData.append('img', this.img[0]);
                const headers = this.getHeaders()


                await axios.post(url, formData, { headers })
                Swal.fire(
                    {
                        icon: 'success',
                        title: 'Registro exitoso:'
                    }
                ).then(() => {
                    // Después de cerrar el diálogo de SweetAlert, recarga la página
                    location.reload();
                });

            } catch (error) {

                Swal.fire(
                    {
                        icon: 'error',
                        title: 'Ha ocurrido un error.'
                    }
                )
            }
        },
        async loadCategories() {
            const url = `${config.api_host}/categories`;
            const headers = this.getHeaders();
            const { data } = await axios.get(url, { headers });
            data.info.forEach(category => {
                this.categories.push(category.nombre);
            });

        }
    }
}



</script>