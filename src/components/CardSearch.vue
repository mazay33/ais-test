<script lang="ts" setup>
import { ref, watch } from "vue";
const emit = defineEmits(["search"]);

const searchTerm = ref("");

const search = () => {
  emit("search", searchTerm.value);
};

watch(searchTerm, () => {
  search();
});
</script>

<template>
  <div class="search">
    <form @submit.prevent>
      <img src="../assets/icons/search.svg" alt="">
      <input
        type="text"
        v-model="searchTerm"
        placeholder="Введите ЖК / корпус / № квартиры / № паркинга"
      />
    </form>
  </div>
</template>

<style lang="scss" scoped>
  form{
    position: relative;
    width: 100%;
    & input{
      position: relative;
      width: 100%;
      padding: 1.2rem 0rem 1.2rem 9rem;
      ::placeholder{
        padding-left: 5.2rem;
      }
    }
    & img{
      position: absolute;
      top: 1.4rem;
      left: 3.4rem;
      z-index: 1;
    }
  }

</style>

