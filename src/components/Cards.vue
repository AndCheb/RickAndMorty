<script setup>
import Card from './Card.vue';

const props = defineProps(['data', 'episodes', 'showCaracters']);

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
  <div v-if="showCaracters.all">
    <p v-for="(item, index) in data" :key="index">
      <div v-if="item.status === 'Alive' && showCaracters.alive">
        <Card :item="item" :episodes="episodes" :show="show"/>
      </div>
      <div v-if="item.status === 'Dead' && showCaracters.dead">
        <Card :item="item" :episodes="episodes" :show="show"/>
      </div>
      <div v-if="item.status === 'unknown' && showCaracters.unknown">
        <Card :item="item" :episodes="episodes" :show="show"/>
      </div>
    </p>
  </div>
  <div v-else>
    <p v-for="(item, index) in data" :key="index">
    <div v-if="item.name.toLowerCase().includes(showCaracters.selectedCaracter.toLowerCase())">
        <Card :item="item" :episodes="episodes" :show="show"/>
      </div>
      </p>
  </div>
  
</template>

<style scoped></style>
