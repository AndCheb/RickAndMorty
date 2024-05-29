<script setup>
const props = defineProps(['item', 'episodes', 'show']);
</script>

<template>
  <img :src="item.image" alt="" />
  <p>{{ item.name }}</p>
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
</template>
