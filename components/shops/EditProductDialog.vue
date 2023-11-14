<template>
    <v-col cols="5">
        <v-btn prepend-icon="mdi-pencil" color="#FFCC00" @click="dialog = true">Editar</v-btn>
    </v-col>

    <v-dialog v-model="dialog" width="60%">
        <v-card>
            <v-toolbar color="#FFCC00" :elevation="8">
                <v-card-title class="card-title">EDITAR PRODUCTO</v-card-title>

            </v-toolbar>

            <v-card-text>
                <form action="javascript:void(0)" @submit="saveProducts">
                    <v-row>
                        <v-col cols="6">

                            <v-text-field label="Nombre" v-model="product.name" required variant="underlined"
                                placeholder="Ingrese el nombre del producto" style="width:100%;" />

                            <v-combobox label="Categoria" :items="categories" v-model="product.category"
                                variant="underlined" style="width:100%;"></v-combobox>

                            <v-combobox label="Marca" :items="brands" v-model="product.brand" variant="underlined"
                                style="width:100%;"></v-combobox>

                            <v-text-field label="Precio" v-model="product.price" required variant="underlined"
                                placeholder="Ingrese el precio del producto" style="width:100%;" type="number" />

                            <v-text-field label="Cantidad" v-model="product.cantida" required variant="underlined"
                                placeholder="Ingrese la cantidad del producto" style="width:100%;" type="number" />

                            <v-file-input label="Imagen" variant="underlined" v-model="product.img" style="width:100%;"
                                accept=".jpg, .jpeg, .png"></v-file-input>

                        </v-col>

                        <v-col cols="6" class="d-flex justify-center align-center">
                            <v-card :elevation="8" height="330" width="230" dark>
                                <v-img></v-img>
                            </v-card>

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

export default {
    data() {
        return {
            dialog: false,
            product: {},
            categories: [],
            brands: [],



        };

    },
    created() {
        this.loadCategories();
        this.loadBrands();
    },
    methods: {
        async saveProducts() {
            const result = await axios.post("http://localhost:3001/product", product.value)
            console.log(result);
        },
        async loadCategories() {
            const url = 'http://localhost:3001/categories';
            const { data } = await axios.get(url);
            data.forEach(category => {
                this.categories.push(category.name);
            });

        },
        async loadBrands() {
            const url = 'http://localhost:3001/brands';
            const { data } = await axios.get(url);
            data.forEach(brand => {
                this.brands.push(brand.name);
            });
        }
    }
}

</script>