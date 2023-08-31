<script setup lang="ts">
import axios from 'axios';
import { ref, onMounted } from 'vue';

const apiKey = '1c014d7470bc87a0ac57d62cc0cf852b';
const ville = ref('Paris');
const nomVille = ref('');
const temp = ref('');
const description = ref('');
const icon = ref('');

onMounted(() => {
  getMeteo();
});



async function getMeteo() {
  const apiUrl = `https://api.openweathermap.org/data/2.5/weather?q=${ville.value}&units=metric&lang=fr&appid=${apiKey}`;

  try {
    const response = await axios.get(apiUrl);
    temp.value = response.data.main.temp + '°C';
    description.value = response.data.weather[0].description;
    icon.value = `https://openweathermap.org/img/wn/${response.data.weather[0].icon}.png`;
    nomVille.value = response.data.name;
  } catch (error) {
    console.error('Une erreur s\'est produite lors de la récupération des données météo', error);
  }
}
</script>

<template>
  <div class="md:p-0">
  <div class="flex flex-col items-center mt-10 gap-10">
    <div class="flex">
    <input v-model="ville" type="text" class=" md:w-80 text-center">
    <button class=" border border-white border-4 p-4  bg-neutral-750 text-white " @click="getMeteo">Valider</button>
  </div>
  
  <div class="bg-slate-200	 mr-10 ml-10  bg-opacity-40 p-16 card rounded-xl	 md:w-[32rem] flex flex-col justify-center items-center gap-5">
  <h1 class="text-white text-5xl md:text-7xl  whitespace-normal text-center	">{{ nomVille }}</h1>
    <p class="text-white text-5xl">{{ temp }}</p>
  </div>
  <div class="bg-slate-200	 bg-opacity-40 p-16 card rounded-xl	 md:w-96 h-10 flex flex-col justify-center items-center gap-1">
    <img :src="icon" alt="Icone météo" class="">
    <p class="text-white text-2xl">{{ description }}</p>
  </div>
  </div>
</div>
</template>

