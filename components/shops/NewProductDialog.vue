<template>
    <v-col cols="1">
        <v-tooltip text="Agregar producto">
            <template v-slot:activator="{ props }">
                <v-btn icon="mdi-plus" color="#FFCC00" class="mr-4" v-bind="props" @click="dialog = true"></v-btn>
            </template>
        </v-tooltip>

    </v-col>
    <v-dialog v-model="dialog" width="60%">
        <v-card>
            <v-toolbar color="#FFCC00" :elevation="8">
                <v-card-title class="card-title">NUEVO PRODUCTO</v-card-title>

            </v-toolbar>

            <v-card-text>
                <form action="javascript:void(0)" enctype="multipart/form-data" @submit="saveProducts">
                    <v-row>
                        <v-col cols="6">

                            <v-text-field label="Nombre" v-model="nombre" required variant="underlined"
                                placeholder="Ingrese el nombre del producto" style="width:100%;" />

                            <v-combobox label="Categoria" :items="categories" v-model="categoria" variant="underlined"
                                style="width:100%;"></v-combobox>

                            <v-combobox label="Marca" :items="brands" v-model="marca" variant="underlined"
                                style="width:100%;"></v-combobox>

                            <v-text-field label="Precio" v-model="precio" required variant="underlined"
                                placeholder="Ingrese el precio del producto" style="width:100%;" type="number" />

                            <v-text-field label="Cantidad" v-model="cantidad" required variant="underlined"
                                placeholder="Ingrese la cantidad del producto" style="width:100%;" type="number" />



                        </v-col>

                        <v-col cols="6">
                            <v-text-field label="Ubicacion bodega" v-model="ubicacion_bodega" required variant="underlined"
                                placeholder="Ingrese el nombre del producto" style="width:100%;" />
                            <v-file-input label="Imagen" variant="underlined" v-model="imagen" style="width:100%;"
                                accept=".jpg, .jpeg, .png" />

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
            product: {},
            categories: [],
            brands: [],
            nombre: '',
            categoria: '',
            marca: '',
            precio: '',
            cantidad: '',
            ubicacion_bodega: '',
            imagen: null,


        };

    },
    created() {
        this.loadCategories();
        this.loadBrands();
    },
    methods: {
        getHeaders() {
            const token = localStorage.getItem('item');
            return { headers: { 'Authorization': `Bearer ${token}` } }

        },
        async getIdUser() {

            const token = localStorage.getItem('token');
            const url = `${config.api_host}/verify`;
            const user = await axios.post(url, { 'token': token })
            const id = user?.data?.info?.id_usuario;
            return id;

        },
        async saveProducts() {
            try {
                const url = `${config.api_host}/products`;
                const formData = new FormData();
                formData.append('id_category', this.categoria);
                formData.append('id_brand', this.marca);
                formData.append('name', this.nombre);
                formData.append('price', this.precio);
                formData.append('id_user', await this.getIdUser());
                formData.append('cant', this.cantidad);
                formData.append('address', this.ubicacion_bodega);
                formData.append('img', this.imagen[0]);
                const headers = this.getHeaders()


                await axios.post(url, formData, { headers })
                Swal.fire(
                    {
                        icon: 'success',
                        title: 'Registro exitoso:'
                    }
                )

            } catch (error) {
                
                Swal.fire(
                    {
                        icon: 'error',
                        title: 'Ha ocurrido un error.'
                    }
                )
            }
        },
        async loadBrands() {
            const url = `${config.api_host}/brands`;
            const headers = this.getHeaders();
            const { data } = await axios.get(url, { headers });
            data.info.forEach(brand => {
                this.brands.push(brand.nombre);
            });

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