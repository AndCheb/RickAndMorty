<script lang="ts" setup>
import Cards from './components/Cards.vue';
import { reactive, ref, watch } from 'vue';

const urlCharacters = 'https://rickandmortyapi.com/api/character';
const urlEpisode = 'https://rickandmortyapi.com/api/episode';

const episodes = ref(null);
const search = ref(null);

const showCaracters = reactive({
  all: true,
  alive: true,
  dead: true,
  unknown: true,
  selectedCaracter: null
});

const data = ref(null);

const currentPage = ref(1);

const getData = async (page) => {
  const response = await fetch(`${urlCharacters}?page=${page}`);
  const { results } = await response.json();
  data.value = results;
};

const getEpisodes = async (url) => {
  const resp1 = await fetch(url);
  const resp2 = await fetch(`${url}?page=2`);
  const resp3 = await fetch(`${url}?page=3`);

  const res1 = await resp1.json();
  const res2 = await resp2.json();
  const res3 = await resp3.json();

  episodes.value = [res1.results, res2.results, res3.results];
};

const searchName = () => {
  showCaracters.all = false;
  showCaracters.selectedCaracter = search.value;
};

getEpisodes(urlEpisode);
getData(currentPage);

watch(currentPage, (newValue) => {
  getData(newValue);
});
</script>

<template>
  <!-- <input type="text" v-model="currentPage" /> -->

  <select v-model="currentPage">
    <option disabled value="">Select page</option>
    <option>1</option>
    <option>2</option>
    <option>38</option>
  </select>

  <input type="text" v-model="search" />
  <button type="button" @click="searchName">Search</button>

  <Cards :data="data" :episodes="episodes" :showCaracters="showCaracters" />
</template>

<style scoped></style>
