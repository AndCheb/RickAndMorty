<script lang="ts" setup>
import Cards from './components/Cards.vue';
import Pagination from './components/Pagination.vue';
import SearchByName from './components/SearchByName.vue';
import Filter from './components/Filter.vue';
import { reactive, ref, watch } from 'vue';

const urlCharacters = 'https://rickandmortyapi.com/api/character';
const urlEpisode = 'https://rickandmortyapi.com/api/episode';

const episodes = ref(null);
const data = ref(null);
const currentPage = ref(1);
const totalPages = ref(0);
const filterValue = ref(null);

const showCharacters = reactive({
  all: true,
  alive: true,
  dead: true,
  unknown: true,
  selectedCharacter: null
});

const getData = async (page) => {
  const response = await fetch(`${urlCharacters}?page=${page}`);
  const { results, info } = await response.json();
  data.value = results;
  totalPages.value = info.pages;
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

const filterCharacters = (value) => {
  filterValue.value = value;

  if (value === 'Alive' || value === 'Dead' || value === 'unknown') {
    showCharacters.selectedCharacter = null;
  }

  if (value === 'All') {
    showCharacters.all = true;
    showCharacters.alive = true;
    showCharacters.dead = true;
    showCharacters.unknown = true;
    showCharacters.selectedCharacter = null;
  } else {
    showCharacters.all = false;
    showCharacters.alive = false;
    showCharacters.dead = false;
    showCharacters.unknown = false;
    showCharacters[value] = true;
  }
};

const searchName = (inputText) => {
  if (inputText !== 'All') {
    showCharacters.selectedCharacter = inputText;
    filterCharacters(null);
  } else {
    showCharacters.selectedCharacter = null;
    filterCharacters('All');
  }
};

const changePage = (value) => {
  currentPage.value = value;
};

getEpisodes(urlEpisode);
getData(currentPage);

watch(currentPage, (newValue) => {
  getData(newValue);
});
</script>

<template>
  <h1 class="title">The Rick and Morty Characters</h1>
  <div class="header">
    <Pagination @page="changePage" :totalPages="totalPages" />
    <SearchByName @searchName="searchName" />
    <Filter :data="data" @filterCharacters="filterCharacters" @searchName="searchName" />
  </div>

  <Cards
    :data="data"
    :episodes="episodes"
    :showCharacters="showCharacters"
    :filterValue="filterValue"
  />
</template>

<style scoped>
.header {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 30px;
}

.title {
  margin-bottom: 30px;
  text-align: center;
}
</style>
