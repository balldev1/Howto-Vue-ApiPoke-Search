<script setup>
import { ref, onMounted, computed } from 'vue';
import axios from 'axios';

const pokeMon = ref(null);
const countPoke = 16;
const searchValue = ref('');

const filterPoke = computed(() =>
  pokeMon.value.filter((product) =>
    product.name.toLowerCase().startsWith(searchValue.value.toLowerCase())
  )
);

onMounted(async () => {
  try {
    const pokemonArray = [];

    for (let i = 1; i <= countPoke; i++) {
      const response = await axios.get(`https://pokeapi.co/api/v2/pokemon/${i}`);
      pokemonArray.push(response.data);
    }

    pokeMon.value = pokemonArray;
    console.log(pokeMon.value);
  } catch (error) {
    console.error('Error in API request', error);
  }
});
</script>

<template>
  <div>
    <input v-model="searchValue" placeholder="Search Pokemon by Name" />
    <ul v-if="pokeMon">
      <li v-for="item of filterPoke" :key="item.name">
        <img :src="item.sprites.front_default" alt="imagePoke" />
        {{ item.name }}
      </li>
      <li v-if="filterPoke.length === 0">No matching results</li>
    </ul>
  </div>
</template>
