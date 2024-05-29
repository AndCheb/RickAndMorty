<script setup>
const props = defineProps(['data', 'episodes']);

function show(allEpisodes, characterEpisode) {
  let count = 0;

  for (let key in allEpisodes) {
    for (let el in allEpisodes[key]) {
      if (count) {
        count = 0;
        return allEpisodes[key][el];
      }

      if (
        typeof allEpisodes[key][el] === 'number' &&
        allEpisodes[key][el] === +characterEpisode[0].slice(-3).replace(/\D/gi, '')
      ) {
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
    <span v-if="item.status == 'Alive'"> !!! </span>
    <span v-else-if="item.status == 'Dead'"> ??? </span>
    <span v-else> &&& </span>
    <span>{{ item.status }} - {{ item.species }}</span>
    <h2>Last known location:</h2>
    <p>{{ item.location.name }}</p>
    <h2>First seen in:</h2>
    <p v-for="(elem, index) in episodes" :key="index">
       {{ show(elem, item.episode) }}
    </p>
  </p>
</template>

<style scoped></style>
