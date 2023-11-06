<template>
    <v-card>
        <v-layout>
            <v-navigation-drawer permanent>
                <v-list>
                    <v-list-item :prepend-avatar="shopImg" :title="shopName" :subtitle="shopEmail"></v-list-item>
                </v-list>

                <v-divider></v-divider>

                <v-list density="compact" nav>
                    <v-list-item prepend-icon="mdi-shopping" title="Productos"
                        @click="navigateTo('products')"></v-list-item>
                    <v-list-item prepend-icon="mdi-sale" title="Promociones" @click="navigateTo('deals')"></v-list-item>
                    <v-list-item prepend-icon="mdi-history" title="Pedidos" @click="navigateTo('orders')"></v-list-item>
                    <v-list-item prepend-icon="mdi-shape" title="Categorias"
                        @click="navigateTo('categories')"></v-list-item>
                    <v-list-item prepend-icon="mdi-collage" title="Marcas" @click="navigateTo('brands')"></v-list-item>
                </v-list>
                <template v-slot:append>
                    <div class="pa-2">
                        <v-list-item prepend-icon="mdi-logout" title="Salir"  href="login/" ></v-list-item>

                    </div>
                </template>
            </v-navigation-drawer>

            <v-main style="height: 100vh;  overflow-y: auto;"></v-main>
            <div v-if="selectedItem === 'products'">
                <v-container fluid fill-height>

                    <ShopsProducts />
                </v-container>
            </div>
            <div v-else-if="selectedItem === 'deals'">
                <!-- Contenido para 'Shared with me' -->
                <p>Este es el contenido de promociones</p>
            </div>
            <div v-else-if="selectedItem === 'orders'">
                <!-- Contenido para 'Starred' -->
                <p>Este es el contenido de pedidos.</p>
            </div>
            <div v-else-if="selectedItem === 'categories'">
                <!-- Contenido para 'Starred' -->
                <p>Este es el contenido de categorias.</p>
            </div>
            <div v-else-if="selectedItem === 'brands'">
                <!-- Contenido para 'Starred' -->
                <p>Este es el contenido de marcas.</p>
            </div>
        </v-layout>
    </v-card>
</template>
<script>
export default {
    data() {
        return {
            selectedItem: 'products', // Item seleccionado por defecto
            shopName: 'H&M',
            shopEmail: 'h&m@hotmail.com',
            shopImg: 'https://logos-marques.com/wp-content/uploads/2021/03/HM-Logo-1968-99.png'
        };
    },
    methods: {
        navigateTo(page) {
            this.selectedItem = page; // Actualiza la página actual al hacer clic en una opción del menú
        }
    }
};
definePageMeta({
    layout: "blank",
});
</script>