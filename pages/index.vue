<script setup >
import axios from 'axios';
import { ref, onMounted } from 'vue';
import Footer from '../components/Layouts/Footer.vue';

const apiKey = '1c014d7470bc87a0ac57d62cc0cf852b';
const ville = ref('Limoges');
const nomVille = ref('');
const temp = ref('');
const description = ref('');
const icon = ref('');
const maxtemp = ref('');
const mintemp = ref('');
const humidity = ref('');
const isSHOWmodal = ref(false);

onMounted(() => {
  getMeteo();
});

async function getMeteo() {
  const apiUrl = `https://api.openweathermap.org/data/2.5/weather?q=${ville.value}&units=metric&lang=fr&appid=${apiKey}`;
  try {
    const response = await axios.get(apiUrl);
    temp.value = (parseInt(response.data.main.temp)) + '°C';
    mintemp.value = (parseInt(response.data.main.temp_min))+ '°C' ;
    maxtemp.value = (parseInt(response.data.main.temp_max))+ '°C' ;
    description.value = response.data.weather[0].description;
    humidity.value = response.data.main.humidity;
    icon.value = `https://openweathermap.org/img/wn/${response.data.weather[0].icon}.png`;
    nomVille.value = response.data.name;
  } catch (error) {
    isSHOWmodal.value = true;
    ville.value = '';
    console.error('Une erreur s\'est produite lors de la récupération des données météo', error);
  }
}
</script>

<template>
  <div class="md:p-0 md:flex md:items-center min-h-screen md:justify-center">
    <div class="flex flex-col items-center mt-10 gap-10">
      <div class="flex items-center">
        <input
          v-model="ville"
          type="text"
          placeholder="Entrez votre ville"
          @keyup.enter="getMeteo"
          class="border border-white border-4 p-4 md:w-80 text-center"
        >
        <button
          class="border border-white border-4 p-4 bg-neutral-800 text-white"
          @click="getMeteo"
        >
          Valider
        </button>
      </div>
      <div class="h-72 mr-10 ml-10 duration-500 bg-neutral-800 p-16 card rounded-xl md:w-[32rem] flex flex-col justify-center items-center gap-5">
        <h1 class="text-white text-5xl md:text-7xl whitespace-normal text-center">{{ nomVille }}</h1>
        <p class="text-white text-5xl">{{ temp }}</p>
      </div>
      <div class="flex flex-col items-center gap-y-10 md:flex-row">
        <div class="h-[5rem] mr-10 ml-10 duration-500 bg-neutral-800 p-16 card rounded-xl w-[18rem] flex flex-col justify-center items-center gap-5">
          <div class="flex gap-6 mt-2">
            <p>
              <svg
                class="text-white"
                width="24"
                height="24"
                viewBox="0 0 24 24"
                fill="none"
                xmlns="http://www.w3.org/2000/svg"
              >
                <path
                  d="M11.0001 3.67157L13.0001 3.67157L13.0001 16.4999L16.2426 13.2574L17.6568 14.6716L12 20.3284L6.34314 14.6716L7.75735 13.2574L11.0001 16.5001L11.0001 3.67157Z"
                  fill="currentColor"
                />
              </svg>
              <p class="text-white text-center">{{ mintemp }}</p>
            </p>
            <p>
              <svg
                class="text-white"
                width="24"
                height="24"
                viewBox="0 0 24 24"
                fill="none"
                xmlns="http://www.w3.org/2000/svg"
              >
                <path
                  d="M17.6568 8.96219L16.2393 10.3731L12.9843 7.10285L12.9706 20.7079L10.9706 20.7059L10.9843 7.13806L7.75404 10.3532L6.34314 8.93572L12.0132 3.29211L17.6568 8.96219Z"
                  fill="currentColor"
                />
              </svg>
              <p class="text-white text-center">{{ maxtemp }}</p>
            </p>
          </div>
          <div>
            <div class="flex">
              <p class="text-white text-center">{{ humidity }} %</p>
              <svg
                class="text-blue-400 ml-2"
                width="24"
                height="24"
                viewBox="0 0 24 24"
                fill="none"
                xmlns="http://www.w3.org/2000/svg"
              >
                <path
                  fill-rule="evenodd"
                  clip-rule="evenodd"
                  d="M6.34315 19.5208C3.21895 16.3966 3.21895 11.3312 6.34315 8.20705L12 2.5502L17.6569 8.20705C20.781 11.3312 20.781 16.3966 17.6569 19.5208C14.5327 22.645 9.46734 22.645 6.34315 19.5208Z"
                  fill="currentColor"
                />
              </svg>
            </div>
          </div>
        </div>
        <div class="bg-neutral-800 p-16 card rounded-xl w-[18rem] h-10 flex flex-col justify-center items-center gap-1">
          <img :src="icon" alt="Icone météo" class="">
          <p class="text-white text-center text-2xl">{{ description }}</p>
        </div>
      </div>
      <Footer />
      <div class="h-20 md:h-0"></div>
    </div>
  </div>
  <Transition>
    <div v-if="isSHOWmodal" class="fixed inset-0 flex items-center justify-center p-10">
      <div class="bg-neutral-900 text-white p-4 h-full md:h-96 md:w-96 flex justify-center gap-6 flex-col items-center rounded-xl shadow-md">
        <img class="w-40" src="~/assets/img/error.gif" alt="">
        <p class="text-xl text-center">La ville saisie n'a pas été trouvée ou est incorrecte</p>
        <button @click="isSHOWmodal = false" class="bg-white rounded-xl text-black h-10 w-28 text-md">D'accord !</button>
      </div>
    </div>
  </Transition>
</template>


