<template>
    <br><br><br>
    <v-carousel cycle hide-delimiters height="300" show-arrows="hover" v-if="dealsLoaded == true">
        <v-carousel-item v-for="deal in deals" :key="deal.id_promocion">
            <v-img :src="`${deal.imagen}`"></v-img>
        </v-carousel-item>
    </v-carousel>
    <v-item-group selected-class="bg-primary">
        <v-container fluid>
            <h1 class="text-center">Productos</h1>
            <br>
            <v-row>
                <v-col v-for="product in products" :key="product.id_producto" cols="cols" md="4">
                    <v-item v-slot="{ selectedClass, toggle }">
                        <v-card class="mx-auto pa-6" height="330" width="230" dark @click="openDialog(product)" color="gray"
                            elevation="18">

                            <v-img :src="`${product.imagen}`" height="210" cover>
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
                            <v-img :src="`${selectedProduct.imagen}`" height="390px" cover
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

                                <v-card-title style="font-size: 20px; font-family: Arial black, serif; font-weight: bold"
                                    class="text-center">
                                    {{ selectedProduct.nombre }}
                                </v-card-title>
                            </v-row>
                            <v-row no-gutters class="justify-center">
                                <v-card-subtitle>
                                    Cantidad disponible: {{ selectedProduct.cantidad || '00' }}
                                </v-card-subtitle>

                                
                            </v-row>

                            <v-row no-gutters class="justify-center">

                                <v-card-title style="font-size: 20px; font-family: Arial black, serif; font-weight: bold">
                                    {{ selectedProduct.precio }}
                                </v-card-title>
                            </v-row>
                            <v-row no-gutters class="justify-center">
                                <v-btn style="background-color: #FFCC00;" variant="text" size="small"
                                    prepend-icon="mdi-cart-minus" @click="">Agregar</v-btn>



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
    name: 'ViewProducts', // Puedes poner el nombre que desees para tu componente
    props: {
        filterC: { type: Number },
        filterB: {type: Number}
    },
    data() {
        return {
            products: [],
            deals: [],
            dialog: false,
            selectedProduct: {},
            dealsLoaded: false

        };
    },
    created() {
        this.loadProducts();
        this.loadDeals();
    },
    watch: {
        filterC(newFilter) {
            if (newFilter) {

                this.filterByCategory(newFilter)
            }
        },
        filterB(newFilter) {
            if (newFilter) {

                this.filterByBrand(newFilter)
            }
        }
    },
    methods: {
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
        async loadDeals() {
            const url = `${config.api_host}/deals`;
            try {
                const headers = this.getHeaders();
                const { data } = await axios.get(url, { headers });
                this.dealsLoaded = true;
                this.deals = data.info;

            } catch (error) {
                console.error('Error al cargar los datos de ofertas:', error);
            }
        },
        openDialog(item) {
            this.selectedProduct = item;
            this.dialog = true;
        },
        async filterByCategory(id_category) {
            console.log(id_category);
            const url = `${config.api_host}/products/category/${id_category}`;
            const headers = this.getHeaders();
            const { data } = await axios.get(url, { headers });
            this.products = data.info;
            // this.loadProducts();
        },
        async filterByBrand(id_brand) {
            const url = `${config.api_host}/products/brand/${id_brand}`;
            const headers = this.getHeaders();
            const { data } = await axios.get(url, { headers });
            this.products = data.info;
            // this.loadProducts();
        }
    }
}
</script>
