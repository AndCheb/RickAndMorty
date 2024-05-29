<script lang="ts" setup>
import Cards from './components/Cards.vue';
import { ref, watch } from 'vue';

const urlCharacters = 'https://rickandmortyapi.com/api/character';
const urlEpisode = 'https://rickandmortyapi.com/api/episode';

const episodes = ref(null);

const getEps = async (url) => {
  const resp1 = await fetch(url);
  const resp2 = await fetch(`${url}?page=2`);
  const resp3 = await fetch(`${url}?page=3`);

  const res1 = await resp1.json();
  const res2 = await resp2.json();
  const res3 = await resp3.json();

  episodes.value = [res1.results, res2.results, res3.results];
}

getEps(urlEpisode);

const data = ref(null);

const currentPage = ref(1);

const getData = async (page) => {
  const response = await fetch(`${urlCharacters}?page=${page}`);
  const { results } = await response.json();
  data.value = results;
};

getData(currentPage);

watch(currentPage, (newValue) => {
  getData(newValue);
});

function show(allEpisodes, characterId, characterEpisode) {
  let count = 0;

  for (let key in allEpisodes) {
    for (let el in allEpisodes[key]) {
      if (count) {
        count = 0;
        return allEpisodes[key][el];
      }

      if (typeof allEpisodes[key][el] === 'number' && allEpisodes[key][el] === +characterEpisode[0].slice(-3).replace(/\D/gi, '')) {
        count++;
      }
    }
  }
}
</script>

<template>
  <p v-for="(item, index) in data" :key="index">
    <img :src="item.image" alt="" />
    <p>{{item.name}}</p>
    <p>{{ item.id }}</p>
    <p v-for="(elem, index) in episodes" :key="index">
      {{ show(elem, item.id, item.episode) }}
    </p>
  </p>
  <Cards />

   <select v-model="currentPage">
    <option disabled value="">Select page</option>
    <option>1</option>
    <option>2</option>
    <option>28</option>
  </select>

  <p>Select: {{ currentPage }}</p> 

</template>

<style scoped></style>
