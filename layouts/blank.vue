<template>
  <div v-if="!loading">

    <!-- slot: Carga el contenido de la página (pages/..) -->
    <slot />

  </div>
</template>
<script setup>
import axios from 'axios'
import config from '../config/default.json'
onBeforeMount(() => {
  const token = localStorage.getItem('token')
  if (token) {
    loading.value = false;
    const url = `${config.api_host}/verify`;
    axios.post(url, { token }).then(()=>{
      loading.value = false;
      
    }).catch(()=>{

      useRouter().push('/login')
    })
  } else {
    useRouter().push('/login')
  }
})
const loading = ref(true)


</script>