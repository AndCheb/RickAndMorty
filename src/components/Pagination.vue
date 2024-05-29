<script setup>
import { ref } from 'vue';

defineEmits(['page']);
defineProps(['totalPages']);

const selected = ref(null);
</script>

<template>
  <label for="select">Select page </label>
  <select v-model="selected" @change="$emit('page', selected)" id="select">
    <option v-for="(item, index) in totalPages" :key="index">
      {{ item }}
    </option>
  </select>
</template>
