<template>
  <h1 :class="{hayUsuarios:hayUsuariosConectados,noHayUsuarios:!hayUsuariosConectados}">Bienvenido</h1>
  <h2 v-if="hayUsuariosConectados">Usuarios conectados: {{ cantidadDeUsuariosConectados }}</h2>
  <h2 v-else >No hay usuario conectados</h2>
  <button :disabled="!hayUsuariosConectados" @click="cambiarWidth">cambiar tamaño imagen</button>

<img v-for="url in urlDogList" :key="index" :src="url" :width="widthImg" height="200">

<input type="text" v-model="nombreDeUsuario">
<h1>{{ nombreDeUsuario }}</h1>

</template>

<script setup>
import { onMounted, ref, computed, watch } from 'vue';
import axios from 'axios'
  const cantidadDeUsuariosConectados = ref(0)
  const esAdministrador = ref(true)
  const widthImg = ref(200)
  const urlDogList = ref([]) 
  const nombreDeUsuario = ref(null)
  const hayUsuariosConectados = computed (()=>urlDogList.value.length > 0)



  const cambiarWidth = ()=> widthImg.value += 20
  onMounted(async ()=>{ 
    
      const response = await axios("https://www.mockachino.com/f591c2ad-c52b-40/conectados")
      cantidadDeUsuariosConectados.value = response.data.conectados
    
      const responseDogs = await axios("https://dog.ceo/api/breeds/image/random/5")
      urlDogList.value = responseDogs.data.message
    

  
  })
 </script>



<style scoped>
  .hayUsuarios{
    color: green;
  }

  .noHayUsuarios{
    color: red;
  }
</style>
