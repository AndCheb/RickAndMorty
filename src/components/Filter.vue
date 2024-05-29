<script setup>
import { ref } from 'vue';

defineProps(['data']);
defineEmits(['filterCharacters', 'searchName']);

const selectedCharacter = ref(null);
const statusCharacter = ref(null);
</script>

<template>
  <select v-model="statusCharacter">
    <option>All</option>
    <option>Alive</option>
    <option>Dead</option>
    <option>unknown</option>
  </select>

  <button type="button" @click="$emit('filterCharacters', statusCharacter)">Filter status</button>

  <select v-model="selectedCharacter">
    <option>All</option>
    <option v-for="(item, index) in data" :key="index">{{ item.name }}</option>
  </select>

  <button type="button" @click="$emit('searchName', selectedCharacter)">Filter characters</button>
</template>
