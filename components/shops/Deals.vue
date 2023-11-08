<template>
    <v-item-group selected-class="bg-primary">
        <v-container fluid>
            <v-row align="center">
                <v-col cols="9">
                    <v-text-field :loading="loading" density="compact" variant="solo" label="Buscar promoción"
                        append-inner-icon="mdi-magnify" single-line hide-details @click:append-inner="onClick"
                        style="width:50%;" class="mx-auto text-center"></v-text-field>
                </v-col>
                <ShopsNewDealDialog />

            </v-row>

            <br>
            <br>
            <v-row>
                <v-col v-for="deal in deals" :key="deal.id" cols="cols" md="4">
                    <v-item v-slot="{ selectedClass, toggle }">
                        <v-card class="mx-auto pa-6" height="330" width="230" dark @click="openDialog(deal)" color="gray"
                            elevation="18">

                            <v-img :src="`_nuxt/assets/img/deals/${deal.img}`" height="210" cover>
                            </v-img>


                            <v-card-title class="text-center" style="font-weight: bold;">{{ deal.description
                            }}</v-card-title>

                        </v-card>
                    </v-item>
                </v-col>
            </v-row>
            <v-dialog v-model="dialog" max-width="700px" max-height="auto">
                <v-card style="border-radius: 20px">
                    <v-toolbar color="#FFCC00" :elevation="8">
                        <v-card-title class="card-title">

                        </v-card-title>

                    </v-toolbar>
                    <v-row no-gutters class="justify-center align-center">
                        <v-col cols="12" class="mt-2 mr-2">
                            <v-img :src="`_nuxt/assets/img/deals/${selectedDeal.img}`" height="390px" class="ml-2"
                                style="border-radius: 20px">
                            </v-img>



                            <v-row no-gutters class="justify-center align-center">

                                <v-card-title style="font-size: 20px; font-family: Arial black, serif; font-weight: bold">
                                    {{ selectedDeal.description }}
                                </v-card-title>
                            </v-row>
                            <v-row no-gutters class="justify-center">
                                <v-card-subtitle>
                                    FECHA INICIO: {{ selectedDeal.start_date || '00' }}
                                </v-card-subtitle>

                                <v-card-subtitle>
                                    FECHA FINALIZACION: {{ selectedDeal.end_date || 'S M L ' }}
                                </v-card-subtitle>
                                <v-card-subtitle>
                                    PORCENTAJE: {{ selectedDeal.porcen || '00' }}
                                </v-card-subtitle>
                            </v-row>
                            <br>

                            <v-row justify="end">
                                <v-col cols="3">
                                    <v-btn prepend-icon="mdi-delete-empty" color="error" @click="deleteDeal()">Eliminar</v-btn>
                                </v-col>
                                <ShopsEditDealDialog />
                            </v-row>
                            <br>



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
    data() {
        return {
            loaded: false,
            loading: false,
            selectedDeal: {},
            deals: [],
            dialog: false,
            overlay: false,
        };
    },
    created() {
        this.loadDeals();
    },
    methods: {
        onClick() {
            this.loading = true

            setTimeout(() => {
                this.loading = false
                this.loaded = true
            }, 2000)
        },
        async loadDeals() {
            const url = 'http://localhost:3001/deals';
            const { data } = await axios.get(url);
            this.deals = data;
            // console.log(this.deals);
        },
        openDialog(item) {
            this.selectedDeal = item;
            this.dialog = true;
        },
        deleteDeal(item){

        },
        editDeal(item){

        }

    }
}
</script>