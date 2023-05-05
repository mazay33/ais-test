<script setup>
import { ref } from "vue";
import Card from "./components/Card.vue";
import CardSearch from "./components/CardSearch.vue";
import cardData from "./composables/cardData";

const searchTerm = ref("");
const handleSearch = (value) => {
  searchTerm.value = value;
};

const allChecked = ref(false);
const updateAllChecked = (isChecked) => {
  allChecked.value = isChecked;
};

const handleAllChecked = (event) => {
  const isChecked = event.target.checked;
  updateAllChecked(isChecked);
};

const uncheckAll = () => {
  allChecked.value = false;
};
</script>

<template>
  <div class="container">
    <div class="search">
      <CardSearch @search="handleSearch" />
    </div>
    <div class="checkbox">
      <input type="checkbox" v-model="allChecked" @change="handleAllChecked" />
      <label for="allChecked">все</label>
      <button @click="uncheckAll">
        <span>Удалить</span>
        <img src="./assets/icons/delete.svg" alt="" />
      </button>
    </div>
    <div class="cards">
      <Card
        :searchTerm="searchTerm"
        :cardData="cardData"
        :allChecked="allChecked"
        @updateAllChecked="updateAllChecked"
      />
    </div>
  </div>
</template>

<style lang="scss" scoped>
.checkbox {
  display: flex;
  align-items: center;
  gap: 1rem;
  margin-top: 2.9rem;
  & button{
    display: flex;
    padding: .7rem 1.3rem; 
    gap: 1.2rem;
    background:#E5E5E5;
    cursor: pointer;
  }
}
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
