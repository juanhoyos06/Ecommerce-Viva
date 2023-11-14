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
                <form action="javascript:void(0)" @submit="saveProducts">
                    <v-row>
                        <v-col cols="6">

                            <v-text-field label="Nombre" v-model="product.nombre" required variant="underlined"
                                placeholder="Ingrese el nombre del producto" style="width:100%;" />

                            <v-combobox label="Categoria" :items="categories" v-model="product.id_categoria"
                                variant="underlined" style="width:100%;"></v-combobox>

                            <v-combobox label="Marca" :items="brands" v-model="product.id_marca" variant="underlined"
                                style="width:100%;"></v-combobox>

                            <v-text-field label="Precio" v-model="product.precio" required variant="underlined"
                                placeholder="Ingrese el precio del producto" style="width:100%;" type="number" />

                            <v-text-field label="Cantidad" v-model="product.cantidad" required variant="underlined"
                                placeholder="Ingrese la cantidad del producto" style="width:100%;" type="number" />



                        </v-col>

                        <v-col cols="6" >
                            <v-text-field label="Ubicacion bodega" v-model="product.ubicacion_bodega" required variant="underlined"
                                placeholder="Ingrese el nombre del producto" style="width:100%;" />
                            <v-file-input label="Imagen" variant="underlined" v-model="product.imagen" style="width:100%;"
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


export default {
    data() {
        return {
            dialog: false,
            product: {},
            categories: [],
            brands: []


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
        getHeaders() {
            const token = localStorage.getItem('item');
            return { headers: { 'Authorization': `Bearer ${token}` } }

        },
        async loadBrands() {
            const url = `${config.api_host}/brands`;
            const headers = this.getHeaders();
            const { data } = await axios.get(url, { headers });
            this.brands = data.info;
            console.log(this.brands);

        },
        async loadCategories() {
            const url = `${config.api_host}/categories`;
            const headers = this.getHeaders();
            const { data } = await axios.get(url, { headers });
            this.categories = data.info;

        }
    }
}

</script>