<template>
    <v-col cols="1">
        <v-tooltip text="Agregar marca">
            <template v-slot:activator="{ props }">
                <v-btn icon="mdi-plus" color="#FFCC00" class="mr-4" v-bind="props" @click="dialog = true"></v-btn>
            </template>
        </v-tooltip>
    </v-col>
    <v-dialog v-model="dialog" width="60%">
        <v-card>
            <v-toolbar color="#FFCC00" :elevation="8">
                <v-card-title class="card-title">NUEVA MARCA</v-card-title>

            </v-toolbar>

            <v-card-text>
                <form action="javascript:void(0)" @submit="saveBrand()">
                    <v-row>
                        <v-col cols="6">

                            <v-text-field label="Nombre" v-model="brand.name" required variant="underlined"
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
    data() {
        return {
            dialog: false,
            brand: {},

        };

    },
    created() {

    },
    methods: {
        getHeaders() {
            const token = localStorage.getItem('token');
            return { headers: { 'Authorization': `Bearer ${token}` } }

        },
        async saveBrand() {
            try {
                const url = `${config.api_host}/brands`
                const headers = this.getHeaders()

                await axios.post(url, this.brand, { headers })
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
                        title: 'Ha ocurrido un error',
                    }
                )
            }
        }
    }
}

</script>