<script setup>
import { ref } from 'vue';

defineEmits(['searchName']);

const inputText = ref('');
</script>

<template>
  <div class="select-container">
    <input type="text" v-model="inputText" placeholder="Enter character name" />
    <button type="button" @click="$emit('searchName', inputText)">Search</button>
  </div>
</template>
