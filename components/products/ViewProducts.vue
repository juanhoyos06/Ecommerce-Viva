<template>
    <br><br><br>
    <v-carousel cycle hide-delimiters height="300" show-arrows="hover" v-if="dealsLoaded == true">
        <v-carousel-item v-for="deal in deals" :key="deal.id_promocion">
            <v-img :src="`${deal.imagen}`"></v-img>
        </v-carousel-item>
    </v-carousel>
    <v-item-group selected-class="bg-primary">
        <v-container fluid>
            <br><br>
            <v-row align="center">
                <v-col cols="12">
                    <v-text-field :loading="loading" density="compact" variant="solo" label="Buscar producto"
                        append-inner-icon="mdi-magnify" single-line hide-details @click:append-inner="onClick"
                        style="width:50%;" class="mx-auto text-center"></v-text-field>
                </v-col>

            </v-row>
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
                        <v-card-title class="card-title">{{ selectedProduct.nombre }}</v-card-title>

                    </v-toolbar>
                    <v-row no-gutters>
                        <v-col cols="6" class="mt-2 mb-2">
                            <v-img :src="`${selectedProduct.imagen}`" height="390px" cover style="border-radius: 20px">
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
                                <v-form>

                                    <v-btn icon="mdi-minus" variant="text" @click="decrementCant()" />
                                    {{ cant }}
                                    <!-- <v-text-field variant="outlined" style="width: 20px;"></v-text-field> -->
                                    <v-btn icon="mdi-plus" variant="text" @click="incrementCant()" />


                                    <v-btn style="background-color: #FFCC00;" variant="text" size="small"
                                        prepend-icon="mdi-cart-minus"
                                        @click="agregarProducto(selectedProduct.id_producto)">Agregar</v-btn>
                                </v-form>



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
import Swal from "sweetalert2";


export default {
    name: 'ViewProducts', // Puedes poner el nombre que desees para tu componente
    props: {
        filterC: { type: Number },
        filterB: { type: Number }
    },
    data() {
        return {
            loaded: false,
            loading: false,
            products: [],
            deals: [],
            dialog: false,
            selectedProduct: {},
            dealsLoaded: false,
            cant: 0,
            carrito: {}

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
        onClick() {
            this.loading = true

            setTimeout(() => {
                this.loading = false
                this.loaded = true
            }, 2000)
        },
        getHeaders() {
            const token = localStorage.getItem('token');
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
            
        },
        async filterByBrand(id_brand) {
            const url = `${config.api_host}/products/brand/${id_brand}`;
            const headers = this.getHeaders();
            const { data } = await axios.get(url, { headers });
            this.products = data.info;
            
        },
        incrementCant() {
            if (this.cant < this.selectedProduct.cantidad) {
                this.cant++

            }

        },
        decrementCant() {
            if (this.cant > 0) {

                this.cant--
            }

        },

        async getIdUser() {

            const token = localStorage.getItem('token');
            const url = `${config.api_host}/verify`;
            const user = await axios.post(url, { 'token': token })
            const id = user?.data?.info?.id_usuario;
            return id;

        },
        async agregarProducto(id) {
            const token = localStorage.getItem('token')
            if (token) {
                const url = `${config.api_host}/verify`;
                axios.post(url, { token }).then(() => {
                    this.addCarro(id)

                }).catch(() => {

                    useRouter().push('/login')
                })
            } else {
                useRouter().push('/login')
            }



        },
        async addCarro(id) {
            try {
                if (this.cant > 0) {

                    const url = `${config.api_host}/users/carrito`;
                    const formData = new FormData();
                    formData.append('id_product', id);
                    formData.append('id_user', await this.getIdUser());
                    formData.append('cant', this.cant);

                    const headers = this.getHeaders()


                    await axios.post(url, formData, { headers })
                    Swal.fire(
                        {
                            icon: 'success',
                            title: 'Registro exitoso:'
                        }
                    ).then(() => {
                        
                    });

                }

            } catch (error) {
                Swal.fire(
                    {
                        icon: 'error',
                        title: 'Ha ocurrido un error.'
                    }
                )
            }
        }
    }
}
</script>
