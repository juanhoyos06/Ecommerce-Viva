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
            <v-dialog v-model="dialog" max-width="700px" max-height="400px">
                <v-card style="border-radius: 20px">
                    <v-card-title class="text-center"
                        style="font-size: 30px; font-family: Times New Roman, serif; font-weight: bold; background-color: black; color: white;">
                        {{ selectedProduct.name }}
                    </v-card-title>
                    <v-row no-gutters>
                        <v-col cols="6">
                            <v-img :src="`_nuxt/assets/img/products/${selectedProduct.image}`" height="auto" cover>
                            </v-img>
                        </v-col>
                        <v-col cols="6" class="d-flex align-center">
                            <v-row no-gutters>
                                <v-card-subtitle>
                                    Marca
                                </v-card-subtitle>

                                <v-card-text>
                                    {{ selectedProduct.brand || 'Sin Marca' }}
                                </v-card-text>
                                <v-card-subtitle>
                                    Categoria
                                </v-card-subtitle>

                                <v-card-text>
                                    {{ selectedProduct.category || 'Sin Categoria' }}
                                </v-card-text>

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
        },
      

    }
}
export const componentMethodsMixin = {
    methods: {
        myExportedMethod(msg) {
            this.products = this.products.filter(product => product.category === msg) 
            loadProducts()
        }
    }
}

</script>
