<script setup lang="ts">
import { ref } from 'vue';

const rating = ref(0);

const handleRatingClick = (selectedRating: number) => {
  if (rating.value === selectedRating) {
    rating.value = 0;
  } else {
    rating.value = selectedRating;
  }
};
</script>

<template>
  <form class="feedback-form">
    <h2 class="feedback-form__title">Форма обратной связи</h2>
    <span class="feedback-form__subtitle">Пожалуйста, оцените свой опыт прохождения тестового</span>
    <div class="rating">
      <div class="rating__items">
        <input
          v-for="star in 5"
          :key="star"
          class="rating__item"
          :id="`rating_${star}`"
          name="rating"
          type="radio"
          :value="star"
          :checked="rating === star"
          @click="handleRatingClick(star)"
        />
        <label
          v-for="star in 5"
          :key="`label_${star}`"
          class="rating__label"
          :for="`rating_${star}`"
          :class="{ 'rating__label--active': star <= rating }"
        >
          ★
        </label>
      </div>
    </div>
  </form>
</template>

<style scoped>
.rating {
  display: flex;
}

.rating__item {
  display: none;
}

.rating__label {
  font-size: 24px;
  color: #ccc;
  cursor: pointer;
  transition: color 0.2s;
}

.rating__label--active {
  color: gold;
}
</style>