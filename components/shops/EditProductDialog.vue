<template>
    <v-col cols="5">
        <v-btn prepend-icon="mdi-pencil" color="#FFCC00" @click="openEditDialog()">Editar</v-btn>
    </v-col>

    <v-dialog v-model="dialog" width="60%">
        <v-card>
            <v-toolbar color="#FFCC00" :elevation="8">
                <v-card-title class="card-title">EDITAR PRODUCTO</v-card-title>

            </v-toolbar>

            <v-card-text>
                <form action="javascript:void(0)" enctype="multipart/form-data" @submit="editProducts()">
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
        selectedProduct: { type: Number },
    },
    data() {
        return {
            dialog: false,
            // idProd: '',
            product: {},
            categories: [],
            brands: [],
            nombre: '',
            categoria: '',
            marca: '',
            precio: '',
            cantidad: '',
            estado: '',
            ubicacion_bodega: '',
            


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
        async editProducts() {
            
            try {
                this.openEditDialog()
                const url = `${config.api_host}/products/${this.$props.selectedProduct}`;
                this.product ={
                    'id_category': this.categoria,
                    'id_brand': this.marca,
                    'name': this.nombre,
                    'price': this.precio,
                    'status': this.estado,
                    'id_user': this.getIdUser(),
                    'cant': this.cantidad,
                    'address': this.ubicacion_bodega
                } 
                const headers = this.getHeaders()

                console.log(this.product);
                await axios.put(url, this.product, { headers })
                Swal.fire(
                    {
                        icon: 'success',
                        title: 'Producto actualizado correctamente'
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
            const productId = this.$props.selectedProduct;
            const url = `${config.api_host}/products/info/${productId}`;
            const headers = this.getHeaders();
            const response = await axios.get(url, { headers });
            const data = response.data.info[0];

            this.nombre = data.nombre;
            this.categoria = data.categoria;
            this.marca = data.marca;
            this.estado = data.estado;
            this.cantidad = data.cantidad;
            this.ubicacion_bodega = data.ubicacion_bodega;
            this.dialog = true;
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
        },
    }
}

</script>