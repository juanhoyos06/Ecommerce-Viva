<template>
    <v-item-group selected-class="bg-primary">
        <v-container fluid>
            <br><br><br>
            <h1 class="text-center">Productos</h1>
            <br>
            <v-row>
                <v-col v-for="product in products" :key="product.id" cols="cols" md="4">
                    <v-item v-slot="{ selectedClass, toggle }">
                        <v-card class="mx-auto pa-6" height="450" width="300" dark @click="openDialog(product)" color="gray"
                            elevation="18">

                            <v-img :src="`_nuxt/assets/img/products/${product.image}`" height="330" cover>
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

                    <v-row no-gutters>
                        <v-col cols="6">
                            <v-img :src="`_nuxt/assets/img/products/${selectedProduct.image}`" height="390px" cover>
                            </v-img>
                        </v-col>
                        <v-col cols="6">
                            <v-row><br><br><br><br><br><br></v-row>
                            <v-row no-gutters class="justify-center">

                                <v-card-title
                                    style="font-size: 25px; font-family: Times New Roman, serif; font-weight: bold">
                                    {{ selectedProduct.name }}
                                </v-card-title>
                            </v-row>
                            <v-row no-gutters class="justify-center">
                                <v-card-subtitle>
                                    Marca: {{ selectedProduct.brand || 'Sin Marca' }}
                                </v-card-subtitle>

                                <v-card-subtitle>
                                    Categoria: {{ selectedProduct.category || 'Sin Categoria' }}
                                </v-card-subtitle>
                            </v-row>
                                <v-row no-gutters class="justify-center">

                                    <v-card-title
                                        style="font-size: 25px; font-family: Times New Roman, serif; font-weight: bold">
                                        ${{ selectedProduct.price }}
                                    </v-card-title>
                                </v-row>
                                <v-row no-gutters class="justify-center">
                                    <v-btn style="background-color: #FFCC00;" variant="text" size="small" prepend-icon="mdi-cart-minus"
                                        @click="">Agregar</v-btn>

                                

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


export const componentMethodsMixin = {

    methods: {
        async myExportedMethod(category) {
            console.log(category, category[0]);
            const url = 'http://localhost:3001/products';
            const { data } = await axios.get(url);
            this.products = data.filter(product => product.category === category[0])
            console.log(this.products);
        }

    }
}
export default {
    name: 'ViewProducts', // Puedes poner el nombre que desees para tu componente
    mixins: [componentMethodsMixin],
    props: {
        filter: { type: Object }
    },
    data() {
        return {
            products: [],
            dialog: false,
            selectedProduct: {},
        };
    },
    created() {
        this.loadProducts();
    },
    watch: {
        filter(newFilter) {
            if (newFilter) {

                this.myExportedMethod(newFilter)
            }
            // console.log(newFilter);

        }
    },
    methods: {
        async loadProducts() {
            const url = 'http://localhost:3001/products';
            const { data } = await axios.get(url);
            console.log(data);
            this.products = data;
        },
        openDialog(item) {
            this.selectedProduct = item;
            this.dialog = true;
        }
    }
}

</script>
