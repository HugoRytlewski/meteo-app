<script setup>
import axios from "axios";
import { ref, onMounted } from "vue";
import Footer from "../components/Layouts/Footer.vue";

const apiKey = "1c014d7470bc87a0ac57d62cc0cf852b";
const ville = ref("Limoges");
const nomVille = ref("");
const temp = ref("");
const description = ref("");
const icon = ref("");
const maxtemp = ref("");
const mintemp = ref("");
const humidity = ref("");
const isSHOWmodal = ref(false);

onMounted(() => {
  getMeteo();
});

async function getMeteo() {
  const apiUrl = `https://api.openweathermap.org/data/2.5/weather?q=${ville.value}&units=metric&lang=fr&appid=${apiKey}`;
  try {
    const response = await axios.get(apiUrl);
    temp.value = parseInt(response.data.main.temp) + "°C";
    mintemp.value = parseInt(response.data.main.temp_min) + "°C";
    maxtemp.value = parseInt(response.data.main.temp_max) + "°C";
    description.value = response.data.weather[0].description;
    console.log(response.data);
    humidity.value = response.data.main.humidity;
    icon.value = `https://openweathermap.org/img/wn/${response.data.weather[0].icon}.png`;
    nomVille.value = response.data.name;
  } catch (error) {
    isSHOWmodal.value = true;
    ville.value = "";
    console.error(
      "Une erreur s'est produite lors de la récupération des données météo",
      error
    );
  }
}
</script>

<template>
  <div class="tkt flex items-center justify-center h-[106vh]">
    <div class="flex flex-col items-center justify-center gap-10">
      <div
        class="color flex justify-center md:h-[40rem] h-[38rem] w-[22rem] overflow-hidden md:w-[30rem]"
      >
        <div class="flex flex-col items-center mt-10">
          <div class="flex">
            <input
              v-model="ville"
              type="text"
              placeholder="Entrez votre ville"
              @keyup.enter="getMeteo"
              class="h-14 rounded-l-[8px] border-white border-4 p-4 md:w-80 text-center"
            />
            <button
              class="flex items-center justify-center rounded-r-[8px] h-14 w-14 bg-btn text-white"
              @click="getMeteo"
            >
              <svg
                class="fill-white h-8"
                clip-rule="evenodd"
                fill-rule="evenodd"
                stroke-linejoin="round"
                stroke-miterlimit="2"
                viewBox="0 0 24 24"
                xmlns="http://www.w3.org/2000/svg"
              >
                <path
                  d="m15.97 17.031c-1.479 1.238-3.384 1.985-5.461 1.985-4.697 0-8.509-3.812-8.509-8.508s3.812-8.508 8.509-8.508c4.695 0 8.508 3.812 8.508 8.508 0 2.078-.747 3.984-1.985 5.461l4.749 4.75c.146.146.219.338.219.531 0 .587-.537.75-.75.75-.192 0-.384-.073-.531-.22zm-5.461-13.53c-3.868 0-7.007 3.14-7.007 7.007s3.139 7.007 7.007 7.007c3.866 0 7.007-3.14 7.007-7.007s-3.141-7.007-7.007-7.007z"
                  fill-rule="nonzero"
                />
              </svg>
            </button>
          </div>

          <div>
            <img class="md:h-64 z-10" src="../assets/img/cloud.png" alt="" />
            <h1 class="z-0 teext text-center translate-y-[-100px]">
              {{ nomVille }}
            </h1>
          </div>

          <div
            class="h-52 flex justify-center text-white text-5xl font-bold items-center w-72 md:w-96 mt-[-3.5rem] colo-bg"
          >
            <p class="">{{ temp }}</p>
            <img :src="icon" alt="Icone météo" class="" />
          </div>
        </div>
        
      </div>
      <div class="fixed bottom-0 relative opacity-40">
            <div class="md:ml-[66px]  flex flex-col justify-center text-center items-center gap-6 md:flex-row">
              <span class="text-sm text-white sm:text-center"
                >© 2023
                <a class=""
                  >Météo ,
                  <a
                    href="https://hugorytlewski.com"
                    target="_blank"
                    class="hover:underline"
                    >Hugo Rytlewski</a
                  >
                  Tous droits réservés</a
                >
              </span>
              <ul
                class="flex flex-wrap items-center mt-3 text-sm font-medium text-white mr-0 gap-4 xl:mr-16 sm:mt-0"
              >
                <li>
                  <a
                    href="https://www.linkedin.com/in/hugo-rytlewski-b06841281/"
                  >
                    <img
                      class="w-6"
                      src="~/assets/img/linkdinwhite.png"
                      alt=""
                    />
                  </a>
                </li>
                <li>
                  <a href="https://github.com/HugoRytlewski">
                    <img
                      class="w-6"
                      src="~/assets/img/githubwhite.png"
                      alt=""
                    />
                  </a>
                </li>
              </ul>
            </div>
          </div>
    </div>
  </div>
  <Transition>
    <div
      v-if="isSHOWmodal"
      class="fixed inset-0 flex items-center justify-center p-10"
    >
      <div
        class="bg-neutral-900 text-white p-4 h-full md:h-96 md:w-96 flex justify-center gap-6 flex-col items-center rounded-xl shadow-md"
      >
        <img class="w-40" src="~/assets/img/error.gif" alt="" />
        <p class="text-xl text-center">
          La ville saisie n'a pas été trouvée ou est incorrecte
        </p>
        <button
          @keyup.enter="isSHOWmodal = false"
          @click="isSHOWmodal = false"
          class="bg-white rounded-xl text-black h-10 w-28 text-md"
        >
          D'accord !
        </button>
      </div>
    </div>
  </Transition>
</template>
