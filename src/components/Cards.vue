<script setup>
import { ref } from 'vue';
const props = defineProps(['data']);
</script>

<template>{{ props.data }}</template>

<style scoped></style>
