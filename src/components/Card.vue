<script setup>
const props = defineProps(['item', 'episodes', 'show']);
</script>

<template>
  <div class="card-wrapper">
    <img class="card-image" :src="item.image" :alt="item.name" width="230" height="220" />
    <div class="card-info">
      <h2 class="card-title">{{ item.name }}</h2>
      <span class="card-alive" v-if="item.status == 'Alive'"></span>
      <span class="card-dead" v-else-if="item.status == 'Dead'"></span>
      <span class="card-unknown" v-else></span>
      <span class="card-status">{{ item.status }} - {{ item.species }}</span>
      <h2 class="card-location">Last known location:</h2>
      <p class="card-location-name">{{ item.location.name }}</p>
      <h2 class="card-location">First seen in:</h2>
      <p class="card-location-name" v-for="(elem, index) in episodes" :key="index">
        {{ show(elem, item.episode) }}
      </p>
    </div>
  </div>
</template>

<style scoped>
.card-wrapper {
  display: flex;
  width: 650px;
  min-height: 220px;
  background-color: var(--color-card-back);
  border-radius: 25px;
}

.card-image {
  display: block;
  height: 100%;
  border-top-left-radius: 25px;
  border-bottom-left-radius: 25px;
  object-fit: cover;
}

.card-info {
  padding-block: 10px;
  padding-inline: 15px;
}

.card-title {
  margin: 0;
  font-size: 25px;
  color: #f5f5f5;
}

.card-alive,
.card-dead {
  display: inline-block;
  margin-right: 7px;
  width: 9px;
  height: 9px;
  background-color: var(--color-green);
  border-radius: 50%;
}

.card-dead {
  background-color: var(--color-red);
}

.card-unknown {
  display: inline-block;
  margin-right: 7px;
  width: 9px;
  height: 9px;
  background-color: #909090;
  border-radius: 50%;
}

.card-status {
  font-weight: 500;
  font-size: 16px;
  text-transform: capitalize;
}

.card-location {
  margin-top: 10px;
  margin-bottom: 5px;
  font-weight: 500;
  font-size: 16px;
  color: #9e9e9e;
}

.card-location-name {
  margin: 0;
  font-size: 18px;
  color: #f5f5f5;
}

@media (max-width: 700px) {
  .card-wrapper {
    flex-direction: column;
    margin: auto;
    width: 420px;
  }

  .card-image {
    width: 100%;
    max-height: 400px;
    border-radius: 25px;
  }
}

@media (max-width: 500px) {
  .card-wrapper {
    width: 100%;
  }
}
</style>
