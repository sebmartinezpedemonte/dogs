<template>
    <div>
      <h1>Lista de personajes de Rick and Morty</h1>
      <input type="text" v-model="busqueda" placeholder="Buscar personaje">
      <ul>
        <li v-for="(personaje, index) in listaFiltrada" :key="index">
          <img v-if="index === 0" :src="personaje.image" :alt="personaje.name" />
          {{ personaje.name }}
        </li>
      </ul>
    </div>
  </template>
  
  <script setup>
  import { ref, computed, onMounted } from 'vue';
  import axios from 'axios';
  
  const busqueda = ref('');
  const personajes = ref([]);
  const listaFiltrada = computed(() => {
    if (!busqueda.value) {
      return personajes.value;
    } else {
      return personajes.value.filter(personaje => {
        return personaje.name.toLowerCase().includes(busqueda.value.toLowerCase());
      });
    }
  });
  
  onMounted(async () => {
    const response = await axios("https://rickandmortyapi.com/api/character/");
    personajes.value = response.data.results.slice(0, 50);
  });
  </script>
  
  <style scoped>
  </style>
  