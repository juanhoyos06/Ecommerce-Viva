<template>
    <v-btn icon="mdi-pencil" variant="text" @click="openEditDialog()"></v-btn>
    <v-dialog v-model="dialog" width="60%">
        <v-card>
            <v-toolbar color="#FFCC00" :elevation="8">
                <v-card-title class="card-title">EDITAR MARCA</v-card-title>

            </v-toolbar>

            <v-card-text>
                <form action="javascript:void(0)" @submit="editBrand()">
                    <v-row>
                        <v-col cols="6">

                            <v-text-field label="Nombre" v-model="name" required variant="underlined"
                                placeholder="Ingrese el nombre de la marca" style="width:100%;" />


                        </v-col>
                        <v-spacer></v-spacer>
                        <v-col cols="2" class="mt-4">
                            <v-btn @click="dialog = false">Cancelar</v-btn>
                        </v-col>
                        <v-col cols="2" class="mt-4">
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
        selectedBrand: {
            type: Number
        }
    },
    data() {
        return {
            dialog: false,
            brand: {},
            name: ''

        };

    },
    created() {

    },
    methods: {
        getHeaders() {
            const token = localStorage.getItem('token');
            return { headers: { 'Authorization': `Bearer ${token}` } }

        },
        async editBrand() {

            try {
                this.openEditDialog()
                const url = `${config.api_host}/brands/${this.$props.selectedBrand}`;
                this.brand = {
                    'name': this.name,

                }
                const headers = this.getHeaders()
                console.log(this.brand);

                await axios.put(url, this.brand, { headers })
                Swal.fire(
                    {
                        icon: 'success',
                        title: 'Marca actualizada correctamente'
                    }
                ).then(() => {
                    // Después de cerrar el diálogo de SweetAlert, recarga la página
                    location.reload();
                });

            } catch (error) {

                Swal.fire(
                    {
                        icon: 'error',
                        title: 'Ha ocurrido un error al actualizar la marca',
                        text: error?.message
                    }
                )
            }
        },
        async openEditDialog() {
            const brandId = this.$props.selectedBrand;
            const url = `${config.api_host}/brands/${brandId}`;
            const headers = this.getHeaders();
            const response = await axios.get(url, { headers });
            const data = response.data.info[0];

            this.name = data.nombre;
            this.dialog = true;

        },
    }
}

</script>