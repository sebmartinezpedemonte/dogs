<template>
<h1>App Cripto</h1>
<button v-if="!ordenarPorNombre" @click="toggleOrden()">Ordenar por Nombre</button>
<button v-else @click="toggleOrden()">Ordenar por Ranking</button>
<h3  :class="{ 'destacada': index < 3 }" v-for="(criptomoneda, index) in criptomonedasOrdenadas" :key="criptomoneda.id">
      {{ criptomoneda.id }} {{ getIndicatorClass(criptomoneda.changePercent24Hr) }}
      
    </h3>

</template>
6. Mostrar un indicador junto a cada criptomoneda en función de la propiedad
changePercent24Hr . Si el valor es positivo, mostrar un ícono de flecha hacia arriba;
si es negativo, mostrar un ícono de flecha hacia abajo.
<script setup>
import { onMounted, ref, computed, watch } from 'vue';
import axios from 'axios'

const criptomonedas = ref([])
const ordenarPorNombre = ref(false)



function getIndicatorClass(changePercent) {
  if (changePercent > 0) {
    
    return "↗"

  } else if (changePercent < 0) {
    return "↘"
  } else {
    return ''
  }
}

const criptomonedasOrdenadas = computed(() => {
  if (ordenarPorNombre.value) {
    return [...criptomonedas.value].sort((a, b) => a.id.localeCompare(b.id))
  } else {
    return [...criptomonedas.value].sort((a, b) => a.ranking - b.ranking)
  }
})

onMounted(async() =>{
  
  const response = await axios("https://api.coincap.io/v2/assets?limit=10")
  criptomonedas.value = response.data.data
})

function toggleOrden() {
  ordenarPorNombre.value = !ordenarPorNombre.value
}
/*
const ordenadaPorRanking = computed(()=> {
  return [...criptomonedas.value].sort((a,b) => a.ranking - b.ranking)
 })

const ordenadaPorId = computed(() => {
  //return [...criptomonedas.value].sort((a,b) => a.id.localeCompare(b.id))
  return [...criptomonedas.value].sort(function (a, b) {
  if (a.id > b.id) {
    return 1;
  }
  if (a.id < b.id) {
    return -1;
  }
  return 0;
})
})
*/
 </script>



<style scoped>

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
Criptomonedas
1. Crear un nuevo componente Vue.js utilizando Vue 3 y la Composition API.
2. Montar este componente como el componente raíz de la aplicación.
3. Utilizar la API de CoinCap (https://api.coincap.io/v2/assets?limit=10) para
obtener una lista de las 10 principales criptomonedas.
4. Presentar la lista de criptomonedas en la vista, ordenada inicialmente por
ranking. Permitir al usuario ordenar la lista también por nombre.
5. Utilizar clases de estilos dinámicas para destacar las tres primeras
criptomonedas en el ranking de alguna manera (por ejemplo, utilizando un color
de fondo diferente).
6. Mostrar un indicador junto a cada criptomoneda en función de la propiedad
changePercent24Hr . Si el valor es positivo, mostrar un ícono de flecha hacia arriba;
si es negativo, mostrar un ícono de flecha hacia abajo.
7. Bonus: Aplicar un estilo verde al indicador si changePercent24Hr es positivo y un
estilo rojo si es negativo.