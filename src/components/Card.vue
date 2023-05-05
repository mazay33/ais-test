<script setup>
import { computed, watch } from "vue";
const props = defineProps({
  cardData: {
    type: Object,
    required: true,
  },
  searchTerm: {
    type: String,
    required: false,
    default: "",
  },
  allChecked: {
    type: Boolean,
    required: false,
    default: "",
  },
});

const filteredData = computed(() => {
  if (!props.searchTerm) {
    return props.cardData.data;
  }
  const term = props.searchTerm.toLowerCase();
  return props.cardData.data.filter((data) => {
    return (
      data.residential_complex?.toLowerCase().includes(term) ||
      data.address?.toLowerCase().includes(term) ||
      data.building_number?.toLowerCase().includes(term) ||
      data.room_number?.toLowerCase().includes(term) ||
      data.object_number?.toLowerCase().includes(term)
    );
  });
});

watch(
  () => props.allChecked,
  (isChecked) => {
    filteredData.value.forEach((data) => {
      data.isChecked = isChecked;
    });
  }
);
</script>

<template>
  <div v-for="data in filteredData" class="card">
    <div class="card__inner">
      <div class="card__checkbox">
        <input type="checkbox"  v-model="data.isChecked" :checked="allChecked" />
      </div>
      <div class="card__top">
        <div class="card__price">{{ data.price }} руб.</div>
        <div
          v-if="data.property_type"
          class="card__category"
        >
        <div v-if="data.property_type == 'Паркинг'" class="card__category_tooltip">Подземная встроенно-пристроенная</div>
          <img
            v-if="data.property_type == 'Паркинг'"
            src="../assets/icons/parking.svg"
            alt=""
          />
          <img
            v-if="data.property_type == 'Квартира'"
            src="../assets/icons/room.svg"
            alt=""
          />
          <span>{{ data.property_type }}</span>
        </div>
        <div
          :class="
            data.seller == 'Уступка от юр. лица'
              ? 'card__type_red'
              : data.seller == 'Уступка от физ. лица'
              ? 'card__type_blue'
              : data.seller == 'Забронировано'
              ? 'card__type_gray'
              : 'card__type_sold'
          "
          class="card__type"
        >
          <span>{{ data.seller }}</span>
        </div>
      </div>
      <div class="card__center">
        <div class="card__left">
          <div class="card__name">
            <span class="card__name_complex"
              >{{ data.residential_complex }},</span
            >
            <span class="card__name_building">
              корпус {{ data.building_number }},
            </span>
            <span class="card__name_completion">
              {{ data.completion_quarter }}</span
            >
          </div>
          <div class="card__info">
            <span v-if="data.room_number" class="card__info_number"
              >кв. {{ data.room_number }}</span
            >
            <span v-if="data.object_number" class="card__info_number">{{
              data.object_number
            }}</span>
            <span v-if="data.total_sq_m" class="card__info_metr"
              >{{ data.total_sq_m }} м²</span
            >
            <span v-if="data.number_of_rooms" class="card__info_quantity"
              >{{ data.number_of_rooms }} комн. кв.</span
            >
            <span v-if="data.floor" class="card__info_floor"
              >{{ data.floor }} этаж</span
            >
          </div>
          <div class="card__address">
            <img src="../assets/icons/location.svg" alt="" />
            <span>{{ data.address }}</span>
          </div>
        </div>

        <div class="card__right">
          <div class="card__picture">
            <img src="../assets/img/img1.png" alt="" />
          </div>
          <div class="card__date">Добавлено 21/11/2020</div>
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.card {
  background: #fff;
  border: 0.1rem solid #e5e5e5;
  max-width: 68.3rem;
  width: 100%;
  &__inner {
    position: relative;
    padding: 2.1rem 3rem 2.5rem 9.2rem;
  }
  &__checkbox {
    position: absolute;
    left: 3.5rem;
    top: 50%;
  }
  &__top {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 0.3rem;
  }
  &__price {
    color: #ff0d29;
    font-size: 1.5rem;
    font-weight: 700;
  }
  &__category {
    position: relative;
    display: flex;
    gap: 1.3rem;
    font-size: 1.2rem;
    padding: 0.5rem 2.2rem;
    background: #ffffff;
    box-shadow: 0rem 0rem 0.2rem rgba(94, 119, 157, 0.25);
    border-radius: 3.2rem;
    cursor: pointer;
    &:hover &_tooltip{
      display: block;
      opacity: .9;
    }
      &_tooltip{
        display: none;
        position: absolute;
        top: 3.2rem;
        left: 0;
        width: 34rem;
        background: #454545;
        color:#fff;
        padding: 1.2rem 2.5rem;
        box-shadow: 0px 8px 30px -3px rgba(26, 32, 44, 0.1), 0px 4px 6px -2px rgba(26, 32, 44, 0.05);
      }
    }
  &__type {
    @media (max-width: 1200px) {
      margin-right: -3rem;
    }
    position: relative;
    padding: 0.6rem 0.9rem 0.6rem 2.3rem;
    &_red {
      background: #ffe4e4;
      color: #dd4c5d;
      &::after {
        content: "";
        position: absolute;
        left: 0.9rem;
        top: 1.3rem;
        width: 0.6rem;
        height: 0.6rem;
        border-radius: 100%;
        background: #dd4c5d;
      }
    }
    &_blue {
      background: #d6f5ff;
      color: #0291c1;
      &::after {
        content: "";
        position: absolute;
        left: 0.9rem;
        top: 1.3rem;
        width: 0.6rem;
        height: 0.6rem;
        border-radius: 100%;
        background: #0291c1;
      }
    }
    &_gray {
      background: #ededed;
      color: #686868;
      &::after {
        content: "";
        position: absolute;
        left: 0.9rem;
        top: 1.3rem;
        width: 0.6rem;
        height: 0.6rem;
        border-radius: 100%;
        background: #686868;
      }
    }
    &_sold {
      background: #cccccc;
      color: #808080;
      &::after {
        content: "";
        position: absolute;
        left: 0.9rem;
        top: 1.3rem;
        width: 0.6rem;
        height: 0.6rem;
        border-radius: 100%;
        background: #808080;
      }
    }
  }
  &__name {
    max-width: 25rem;
    width: 100%;
    align-items: baseline;
    &_building {
      color: #808080;
    }
    &_completion {
      color: #808080;
    }
  }
  &__center {
    display: flex;
    justify-content: space-between;
  }
  &__left {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
  }
  &__bottom {
    display: flex;
    justify-content: space-between;
  }
  &__address {
    position: relative;
    max-width: 30.4rem;
    width: 100%;
    & img {
      position: absolute;
      left: -2.2rem;
      top: 0.3rem;
    }
  }
  &__info {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
  }
  &__picture {
    margin-top: 2.7rem;
    margin-bottom: 1.7rem;
  }
  &__date {
    font-size: 1.3rem;
    color: #9b9b9b;
    text-align: right;
  }
}
</style>
