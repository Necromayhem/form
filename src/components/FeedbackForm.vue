<script setup lang="ts">
import { ref } from 'vue';
import StarRating from './ui/StarRating.vue';
import OptionButtonGroup from './ui/OptionButtonGroup.vue';

const rating = ref(0);

const options = ref([
  { text: 'Интересно', active: false },
  { text: 'Легко', active: false },
  { text: 'Быстро сделал', active: false },
  { text: 'Красиво', active: false },
  { text: 'Подробно описано', active: false },
  { text: 'Все понятно и по делу', active: false }
]);

const handleRatingUpdate = (value: number) => {
  if (rating.value === value) {
    rating.value = 0;
    options.value.forEach(option => option.active = false);
  } else {
    rating.value = value;
  }
};

const handleOptionsUpdate = (newOptions: typeof options.value) => {
  options.value = newOptions;
};
</script>

<template>
  <form class="feedback-form">
    <h2 class="feedback-form__title">Форма обратной связи</h2>
    <span class="feedback-form__subtitle">Пожалуйста, оцените свой опыт прохождения тестового</span>
    
    <StarRating 
      :modelValue="rating" 
      @update:modelValue="handleRatingUpdate" 
    />
    
    <OptionButtonGroup 
      v-if="rating > 0"
      :options="options"
      @update:options="handleOptionsUpdate"
    />
  </form>
</template>

<style scoped>
.feedback-form {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
}

.feedback-form__title {
  margin-bottom: 10px;
  font-size: 24px;
}

.feedback-form__subtitle {
  display: block;
  margin-bottom: 20px;
  color: #666;
}
</style>