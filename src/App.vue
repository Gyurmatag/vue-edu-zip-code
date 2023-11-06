<template>
  <div class="min-h-screen bg-gray-100 flex items-center justify-center">
    <div class="bg-white p-8 flex flex-col space-y-4 rounded-xl shadow-md w-96">
      <input
        placeholder="Zip Code"
        class="w-full p-2 border rounded-md focus:outline-none focus:ring focus:ring-blue-200 transition"
        v-model="zipCode"
      />
      <input
        placeholder="City"
        class="w-full p-2 border rounded-md focus:outline-none focus:ring focus:ring-blue-200 transition"
        v-model="city"
        disabled
      />
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, watch } from "vue";
import { useDebounceFn } from "@vueuse/core";
import axios from "axios";

interface Country {
  id: number;
  name: string;
}

interface Zip {
  id: number;
  country_id: number;
  zip: string;
  name: string;
  lat: number;
  lng: number;
  country: Country;
}

interface Zips {
  zips: Zip[];
}

const zipCode = ref("");
const city = ref("");

const debuncedFn = useDebounceFn(
  () => {
    city.value = "";
    if (zipCode.value) {
      axios
        .get<Zips>(`https://hur.webmania.cc/zips/${zipCode.value}.json`)
        .then((response) => {
          city.value = response.data.zips?.at(0)?.name ?? "";
        });
    }
  },
  500,
  { maxWait: 5000 }
);

watch(zipCode, async () => {
  debuncedFn();
});
</script>
