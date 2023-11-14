<template>
    <v-item-group selected-class="bg-primary">
        <v-container fluid>
            <v-row align="center">
                <v-col cols="9">
                    <v-text-field :loading="loading" density="compact" variant="solo" label="Buscar producto"
                        append-inner-icon="mdi-magnify" single-line hide-details @click:append-inner="onClick"
                        style="width:50%;" class="mx-auto text-center"></v-text-field>
                </v-col>
                <ShopsNewProductDialog />

            </v-row>

            <br>
            <br>
            <v-row>
                <v-col v-for="product in products" :key="product.id_producto" cols="cols" md="4">
                    <v-item v-slot="{ selectedClass, toggle }">
                        <v-card class="mx-auto pa-6" height="330" width="230" dark @click="openDialog(product)" color="gray"
                            elevation="18">

                            <v-img :src="`_nuxt/assets/img/products/${product.imagen}`" height="210" cover>
                            </v-img>


                            <v-card-title class="text-center">{{ product.nombre }}</v-card-title>
                            <v-card-title class="text-center" style="font-weight: bold;">{{ product.precio
                            }}</v-card-title>
                        </v-card>
                    </v-item>
                </v-col>
            </v-row>
            <v-dialog v-model="dialog" max-width="700px" max-height="auto">
                <v-card style="border-radius: 20px">
                    <v-toolbar color="#FFCC00" :elevation="8">
                        <v-card-title class="card-title"></v-card-title>

                    </v-toolbar>
                    <v-row no-gutters>
                        <v-col cols="6" class="mt-2">
                            <v-img :src="`_nuxt/assets/img/products/${selectedProduct.imagen}`" height="390px" cover
                                style="border-radius: 20px">
                            </v-img>
                        </v-col>
                        <v-col cols="6">
                            <v-row><br><br><br><br><br><br></v-row>
                            <v-row no-gutters class="justify-center">
                                <v-card-subtitle>
                                    Marca: {{ selectedProduct.marca || 'Sin Marca' }}
                                </v-card-subtitle>

                                <v-card-subtitle>
                                    Categoria: {{ selectedProduct.categoria || 'Sin Categoria' }}
                                </v-card-subtitle>
                            </v-row>
                            <v-row no-gutters class="justify-center">

                                <v-card-title style="font-size: 20px; font-family: Arial black, serif; font-weight: bold">
                                    {{ selectedProduct.nombre }}
                                </v-card-title>
                            </v-row>
                            <v-row no-gutters class="justify-center">
                                <v-card-subtitle>
                                    Cantidad disponible: {{ selectedProduct.cantidad || '00' }}
                                </v-card-subtitle>

                                <v-card-subtitle>
                                    Tallas: {{ selectedProduct.tall || 'S M L ' }}
                                </v-card-subtitle>
                            </v-row>

                            <v-row no-gutters class="justify-center">

                                <v-card-title style="font-size: 20px; font-family: Arial black, serif; font-weight: bold">
                                    {{ selectedProduct.precio }}
                                </v-card-title>
                            </v-row>
                            <v-row justify="center" class="mr-2">
                                <v-col cols="5">
                                    <v-btn prepend-icon="mdi-delete-empty" color="error"
                                        @click="deleteProduct()">Eliminar</v-btn>
                                </v-col>
                                <ShopsEditProductDialog />
                            </v-row>
                        </v-col>
                    </v-row>
                </v-card>
            </v-dialog>
        </v-container>
    </v-item-group>
</template>
<script>
import axios from "axios";
import config from '../../config/default.json';


export default {
    data() {
        return {
            loaded: false,
            loading: false,
            selectedProduct: {},
            products: [],
            dialog: false,
            overlay: false,
        };
    },
    created() {
        this.loadProducts();
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
            const token = localStorage.getItem('item');
            return { headers: { 'Authorization': `Bearer ${token}` } }

        },

        async loadProducts() {

            const url = `${config.api_host}/products`;
            const headers = this.getHeaders();
            const { data } = await axios.get(url, { headers });
            this.products = data.info;

        },
        openDialog(item) {
            this.selectedProduct = item;
            this.dialog = true;
        },
        deleteProduct() {

        },
        editProduct() {

        }
    }
}
</script>