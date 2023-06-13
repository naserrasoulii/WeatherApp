
<template>
  <section class="w-screen h-screen bg-zinc-500 flex items-center justify-center">
    <div class="bg-slate-200 w-80 text-center rounded shadow overflow-hidden">
      <img src="./assets/cityimage.webp" class="mb-4 w-full" />
      <div class="px-10 mb-4">
        <input type="text" v-model="cityName"
          class="block w-full mb-2 bg-zinc-500 text-white text-center rounded-full py-2 border-1 border-transparent placeholder:text-white/50"
          placeholder="نام شهر را وارد کنید!" />
        <button @click="getData" class="block w-full bg-orange-600 text-white py-2 rounded-full">
          <span v-if="!loading">
            جستجو
          </span>
          <svg v-else class="animate-spin mx-auto h-5 w-5 text-white text-center" xmlns="http://www.w3.org/2000/svg"
            fill="none" viewBox="0 0 24 24">
            <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
            <path class="opacity-75" fill="currentColor"
              d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z">
            </path>
          </svg>
        </button>
        <div class="flex flex-col mt-4" v-if="infoWeather">
          <p>
            نام شهر : {{ infoWeather.name }}
          </p>
          <p>
            وضعیت : {{ infoWeather.weather[0].description }}
          </p>
          <p>
            دما : {{ infoWeather.main.temp }}
          </p>

        </div>
        <div class="flex flex-col mt-4" v-if="errors">
          <p class="text-red-500">
            خطا : {{ errors }}
          </p>
        </div>

      </div>
    </div>
  </section>
</template>
<script setup>
import { ref } from 'vue';
import axios from 'axios';
const cityName = ref('')
const apiKey = '6e16b6a3d523ddc350bc65047b746039'
const infoWeather = ref(null)
const loading = ref(false)
const errors = ref(null)
async function getData() {
  if (cityName.value) {
    errors.value = null
    infoWeather.value = null
    loading.value = true
    axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${cityName.value}&appid=${apiKey}&lang=fa&units=metric`).then(({ data }) => {
      console.log(data)
      infoWeather.value = data
      loading.value = false
      cityName.value = ''
    }).catch((err) => {
      console.log(err)
      loading.value = false
      errors.value = 'شهر یافت نشد!'
      cityName.value = ''
    })
  }
}
</script>