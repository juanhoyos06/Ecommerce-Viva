<template>
    <v-col cols="3">
        <v-btn prepend-icon="mdi-pencil" color="#FFCC00" @click="dialog = true">Editar</v-btn>
    </v-col>
    <v-dialog v-model="dialog" width="60%">
        <v-card>
            <v-toolbar color="#FFCC00" :elevation="8">
                <v-card-title class="card-title">EDITAR PROMOCIÓN</v-card-title>

            </v-toolbar>

            <v-card-text>
                <form action="javascript:void(0)" @submit="saveDeal">
                    <v-row>
                        <v-col cols="6">

                            <v-text-field label="Descripcion" v-model="deal.description" required variant="underlined"
                                placeholder="Ingrese la descripción de la promocion" style="width:100%;" />


                            <v-text-field label="Fecha inicio" v-model="deal.start_date" variant="underlined"
                                style="width:100%;" type="date" />

                            <v-text-field label="Fecha finalización" v-model="deal.end_date" variant="underlined"
                                style="width:100%;" type="date" />

                            <v-text-field label="Porcentaje" v-model="deal.porcen" required variant="underlined"
                                placeholder="Ingrese el porcentaje de descuento" style="width:100%;" type="number" />


                            <v-combobox label="Categoria" :items="categories" v-model="deal.category" variant="underlined"
                                style="width:100%;"></v-combobox>

                            <v-file-input label="Imagen" variant="underlined" v-model="deal.img" style="width:100%;"
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
            deal: {},
            categories: [],
            brands: []


        };

    },
    created() {
        this.loadCategories();
        this.loadBrands();
    },
    methods: {
        async saveDeal() {
            const result = await axios.post("http://localhost:3001/deal", deal.value)
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