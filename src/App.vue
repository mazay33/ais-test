<script setup>
import { ref } from 'vue'
import Card from './components/Card.vue';
import CardSearch from './components/CardSearch.vue'
import cardData from './composables/cardData'


const searchTerm = ref('');
const handleSearch = (value) => {
  searchTerm.value = value;
};

</script>

<template>
  <div class="container">
    <div class="search">
      <CardSearch @search="handleSearch" />
    </div>
    <div class="cards">
      <Card :searchTerm="searchTerm" :cardData="cardData" :selectAllCards="selectAllCards" />
    </div>
  </div>
</template>

<style lang="scss" scoped>
.search {
  margin-top: 9rem;
}
.cards {
  margin-top: 3.6rem;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  row-gap: 2.3rem;
  column-gap: 3.2rem;
  @media (max-width: 1000px) {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
}
</style>
