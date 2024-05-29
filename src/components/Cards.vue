<script setup>
import Card from './Card.vue';

const props = defineProps(['data', 'episodes', 'showCharacters', 'filterValue']);

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
  <div class="cards-wrapper" v-if="showCharacters.all">
    <div v-for="(item, index) in data" :key="index">
      <Card
        v-if="item.status === 'Alive' && showCharacters.alive"
        :item="item"
        :episodes="episodes"
        :show="show"
      />
      <Card
        v-if="item.status === 'Dead' && showCharacters.dead"
        :item="item"
        :episodes="episodes"
        :show="show"
      />
      <Card
        v-if="item.status === 'unknown' && showCharacters.unknown"
        :item="item"
        :episodes="episodes"
        :show="show"
      />
    </div>
  </div>
  <div class="cards-wrapper" v-else>
    <div v-for="(item, index) in data" :key="index">
      <Card
        v-if="item.name.toLowerCase().includes(showCharacters.selectedCharacter?.toLowerCase())"
        :item="item"
        :episodes="episodes"
        :show="show"
      />

      <Card v-if="item.status === filterValue" :item="item" :episodes="episodes" :show="show" />
    </div>
  </div>
</template>

<style>
.cards-wrapper {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
  gap: 30px;
  padding-block: 50px;
}

div:empty {
  display: none;
}
</style>
