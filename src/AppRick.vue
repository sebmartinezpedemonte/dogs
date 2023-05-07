<template>
<h1>Rick and Morty</h1>

<input type="text" v-model="busqueda" placeholder="Buscar personaje"> 

<div v-for="personaje in listaFiltrada" :key="index" >
    <h3 >{{ personaje.name }}</h3> 
    <img :src="personaje.image">
</div> 



</template>

<script setup>
import { onMounted, ref, computed } from 'vue';
import axios from 'axios'

const busqueda = ref('')
const personajes = ref([])
const listaFiltrada = computed(() => {
if(!busqueda.value){
    return personajes.value
}else{
    return personajes.value.filter(personaje => {
        return personaje.name.toLowerCase().includes(busqueda.value.toLowerCase())
    })
}


})

onMounted(async ()=> {

    const responseCharacters = await axios("https://rickandmortyapi.com/api/character/") //https://rickandmortyapi.com/api/character/1,183
    personajes.value =responseCharacters.data.results.slice(0, 50);
})
</script>

/*
Rick and Morty
1. Crear un nuevo componente Vue.js utilizando Vue 3 y la Composition API.
2. Montar este componente como el componente raíz de la aplicación.
3. Utilizar la API de Rick and Morty (https://rickandmortyapi.com/api) para
obtener una lista de 50 objetos (personajes). Consultar la documentación de la
API para conocer cómo hacerlo.
4. Crear un campo de entrada (input) en la interfaz de usuario para permitir la
búsqueda de personajes por nombre o cualquier otra propiedad que prefieras.
5. Utilizar la Composition API para crear una propiedad computada que tome la
cadena de búsqueda ingresada y devuelva un array de personajes filtrado en
función de esa búsqueda.
6. Mostrar la lista filtrada de personajes (propiedad computada) en la vista del
componente.
*/