<template>
    <div>
      <h1>App Cripto</h1>
      <button v-if="!ordenarPorNombre" @click="toggleOrden()">Ordenar por Nombre</button>
    <button v-else @click="toggleOrden()">Ordenar por Ranking</button>
      <div v-for="(criptomoneda, index) in criptomonedasOrdenadas" :key="criptomoneda.id" :class="{ 'destacada': index < 3 }">
        <span>{{ criptomoneda.id }}</span>
        <i :class="getIndicatorClass(criptomoneda.changePercent24Hr)">{{ getIndicatorClass(criptomoneda.changePercent24Hr) }}</i>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted, computed } from 'vue';
  import axios from 'axios';
  
  const criptomonedas = ref([]);
  const ordenarPorNombre = ref(false);
  
  const criptomonedasOrdenadas = computed(() => {
    if (ordenarPorNombre.value) {
      return [...criptomonedas.value].sort((a, b) => a.id.localeCompare(b.id));
    } else {
      return [...criptomonedas.value].sort((a, b) => a.ranking - b.ranking);
    }
  });
  
  onMounted(async () => {
    try {
      const response = await axios.get('https://api.coincap.io/v2/assets?limit=10');
      criptomonedas.value = response.data.data;
    } catch (error) {
      console.log(error);
    }
  });
  
  function toggleOrden() {
    ordenarPorNombre.value = !ordenarPorNombre.value;
  }
  
  function getIndicatorClass(changePercent) {
    if (changePercent > 0) {
      return '↗';
    } else if (changePercent < 0) {
      return '↘';
    } else {
      return '';
    }
  }
  </script>
  
  <style>
  .destacada {
    background-color: yellow;
  }
  
  .indicator-up {
    color: green;
    margin-left: 5px;
  }
  
  .indicator-down {
    color: red;
    margin-left: 5px;
  }
  </style>
  