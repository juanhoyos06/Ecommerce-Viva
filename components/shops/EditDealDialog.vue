<template>
    <v-col cols="3">
        <v-btn prepend-icon="mdi-pencil" color="#FFCC00" @click="openEditDialog()">Editar</v-btn>
    </v-col>
    <v-dialog v-model="dialog" width="60%">
        <v-card>
            <v-toolbar color="#FFCC00" :elevation="8">
                <v-card-title class="card-title">EDITAR PROMOCIÓN</v-card-title>

            </v-toolbar>

            <v-card-text>
                <form action="javascript:void(0)" enctype="multipart/form-data" @submit="editDeals">
                    <v-row>
                        <v-col cols="6">

                            <v-text-field label="Descripcion" v-model="description" required variant="underlined"
                                placeholder="Ingrese la descripción de la promocion" style="width:100%;" />


                            <v-text-field label="Fecha inicio" v-model="start_date" variant="underlined"
                                style="width:100%;" type="date" />

                            <v-text-field label="Fecha finalización" v-model="end_date" variant="underlined"
                                style="width:100%;" type="date" />

                            <v-text-field label="Porcentaje" v-model="percentage" required variant="underlined"
                                placeholder="Ingrese el porcentaje de descuento" style="width:100%;" type="number" />

                        </v-col>

                        <v-col cols="6">
                            <v-combobox label="Categoria" :items="categories" v-model="category" variant="underlined"
                                style="width:100%;"></v-combobox>
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
    props: {
        selectedDeal: { type: Number },
    },
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
        async editDeals() {

            try {
                this.openEditDialog()
                const url = `${config.api_host}/deals/${this.$props.selectedDeal}`;
                this.deal = {
                    'description': this.description,
                    'id_category': this.category,
                    'startDate': this.start_date,
                    'endDate': this.end_date,
                    'id_user': await this.getIdUser(),
                    'percentage': this.percentage,
                    'img': this.img[0],
                }
                const headers = this.getHeaders()

                console.log(this.deal);
                await axios.put(url, this.deal, { headers })
                Swal.fire(
                    {
                        icon: 'success',
                        title: 'Promocion actualizado correctamente'
                    }
                ).then(() => {
                    // Después de cerrar el diálogo de SweetAlert, recarga la página
                    location.reload();
                });

            } catch (error) {

                Swal.fire(
                    {
                        icon: 'error',
                        title: 'Ha ocurrido un error al actualizar el producto.',
                        text: error?.message
                    }
                )
            }
        },
        async openEditDialog() {
            const dealsId = this.$props.selectedDeal;
            const url = `${config.api_host}/deals/info/${dealsId}`;
            const headers = this.getHeaders();
            const response = await axios.get(url, { headers });
            const data = response.data.info[0];
            

            this.description = data.descripcion;
            this.category = data.categoria;
            this.start_date = data.fecha_inicio.slice(0, 10);
            this.end_date = data.fecha_fin.slice(0, 10);
            this.percentage = data.porcentaje;
            this.dialog = true;

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