<template>
    <div>
        <v-sheet rounded="xl" elevation="12" max-width="400" width="100%" class="ma-10 pa-5 mx-auto">
            <v-btn href="/login"
            variant="text" 
            class="position-absolute top-0 left-2" 
            size="x-large"
            icon="mdi-arrow-left"></v-btn>
            <center>
                <img src="~/assets/img/logo_viva_login.png" width="200" height="200" class="ml-20">
            </center>
            <br>
            <v-sheet width="300" class="mx-auto">
                <v-form fast-fail @submit.prevent="">

                    <v-text-field v-model="name" label="Nombre" prepend-icon="mdi-account"></v-text-field>
                    <v-text-field v-model="email" label="Correo" prepend-icon="mdi-email"></v-text-field>
                    <v-text-field v-model="password" label="Contraseña" prepend-icon="mdi-key-variant"></v-text-field>
                    <v-text-field label="Confirmar Contraseña" prepend-icon="mdi-key-variant"></v-text-field>
                    <v-checkbox v-model="conditions">
                        <template v-slot:label>
                            <a href="http://google.com" target="_blank">Acepto terminos y condiciones</a>
                        </template>
                    </v-checkbox>

                    <br>
                    <v-btn type="submit" color="black" block class="mt-2">Guardar</v-btn>

                </v-form>
                <div class="mt-2">
                    <p class="text-body-2">¿Ya tienes cuenta?
                        <NuxtLink to="/login">
                        Inicia sesión
                        </NuxtLink>
                    </p>
                </div>
                <br>
            </v-sheet>
        </v-sheet>
    </div>
</template>
<script>
definePageMeta({
    layout: "blank",
});
const name = ref("")
const email = ref("")
const password = ref("")
const conditions = ref(false)

</script>