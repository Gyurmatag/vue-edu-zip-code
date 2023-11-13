<template>
  <div class="min-h-screen bg-gray-100 flex items-center justify-center">
    <div class="bg-white p-8 flex flex-col space-y-4 rounded-xl shadow-md w-96">
      <input
        v-model="zip"
        placeholder="Zip Code"
        class="w-full p-2 border rounded-md focus:outline-none focus:ring focus:ring-blue-200 transition"
      />
      <input
        placeholder="City"
        class="w-full p-2 border rounded-md focus:outline-none focus:ring focus:ring-blue-200 transition"
        :value="city"
      />
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, watch } from 'vue';
import { useDebounceFn } from '@vueuse/core'
import axios   from 'axios'

let zip = ref('')
let city = ref('')

watch(zip, () => {
  fetchCity()
})

const fetchCity = useDebounceFn(async () => {
  try {
    if(!zip.value) {
      return
    }

    const response = await axios.get(`https://testa.free.beeceptor.com/zips/${zip.value}.json`)

    city.value = response.data.zips?.[0]?.name ?? '';
  } catch (error) {
    city.value = error.message
  }
}, 400, {maxWait: 5000})



</script>
