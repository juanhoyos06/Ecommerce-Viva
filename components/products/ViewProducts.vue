<template>
    <br><br><br>
    <v-carousel cycle hide-delimiters height="300" show-arrows="hover" v-if="dealsLoaded == true">
        <v-carousel-item v-for="deal in deals" :key="deal.id">
            <v-img :src="`_nuxt/assets/img/deals/${deal.img}`"></v-img>
        </v-carousel-item>
    </v-carousel>
    <v-item-group selected-class="bg-primary">
        <v-container fluid>
            <h1 class="text-center">Productos</h1>
            <br>
            <v-row>
                <v-col v-for="product in products" :key="product.id" cols="cols" md="4">
                    <v-item v-slot="{ selectedClass, toggle }">
                        <v-card class="mx-auto pa-6" height="330" width="230" dark @click="openDialog(product)" color="gray"
                            elevation="18">

                            <v-img :src="`_nuxt/assets/img/products/${product.image}`" height="210" cover>
                            </v-img>


                            <v-card-title class="text-center">{{ product.name }}</v-card-title>
                            <v-card-title class="text-center" style="font-weight: bold;">{{ '$' + product.price
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
                            <v-img :src="`_nuxt/assets/img/products/${selectedProduct.image}`" height="390px" cover  style="border-radius: 20px">
                            </v-img>
                        </v-col>
                        <v-col cols="6">
                            <v-row><br><br><br><br><br><br></v-row>
                            <v-row no-gutters class="justify-center">
                                <v-card-subtitle>
                                    Marca: {{ selectedProduct.brand || 'Sin Marca' }}
                                </v-card-subtitle>

                                <v-card-subtitle>
                                    Categoria: {{ selectedProduct.category || 'Sin Categoria' }}
                                </v-card-subtitle>
                            </v-row>
                            <v-row no-gutters class="justify-center">

                                <v-card-title style="font-size: 20px; font-family: Arial black, serif; font-weight: bold" class="text-center">
                                    {{ selectedProduct.name }}
                                </v-card-title>
                            </v-row>
                            <v-row no-gutters class="justify-center">
                                <v-card-subtitle>
                                    Cantidad disponible: {{ selectedProduct.cant || '00' }}
                                </v-card-subtitle>

                                <v-card-subtitle>
                                    Tallas: {{ selectedProduct.tall || 'S M L ' }}
                                </v-card-subtitle>
                            </v-row>

                            <v-row no-gutters class="justify-center">

                                <v-card-title style="font-size: 20px; font-family: Arial black, serif; font-weight: bold">
                                    ${{ selectedProduct.price }}
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

export default {
    name: 'ViewProducts', // Puedes poner el nombre que desees para tu componente
    props: {
        filter: { type: Object }
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
        this.loadDeals();
        this.loadProducts();
    },
    watch: {
        filter(newFilter) {
            if (newFilter) {

                this.filterByCategory(newFilter)
            }
            // console.log(newFilter);

        }
    },
    methods: {
        async loadProducts() {
            const url = 'http://localhost:3001/products';
            const { data } = await axios.get(url);
            this.products = data;
        },
        async loadDeals() {
            const url = 'http://localhost:3001/deals';
            try {
                const { data } = await axios.get(url);
                this.deals = data;
                this.dealsLoaded = true;

            } catch (error) {
                console.error('Error al cargar los datos de ofertas:', error);
            }
        },
        openDialog(item) {
            this.selectedProduct = item;
            this.dialog = true;
        },
        async filterByCategory(category) {
            const url = 'http://localhost:3001/products';
            const { data } = await axios.get(url);
            this.products = data.filter(product => product.category === category[0])
        }
    }
}

</script>
