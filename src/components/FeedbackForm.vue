<script setup lang="ts">
import { ref } from 'vue';
import { useForm } from 'vee-validate';
import StarRating from './ui/StarRating.vue';
import OptionButtonGroup from './ui/OptionButtonGroup.vue';
import FullNameInput from './ui/FullNameInput.vue';
import EmailInput from './ui/EmailInput.vue';
import PhoneInput from './ui/PhoneInput.vue';
import GradeSelect from './ui/GradeSelect.vue';

const rating = ref(0);
const showError = ref(false);

const options = ref([
  { text: 'Интересно', active: false },
  { text: 'Легко', active: false },
  { text: 'Быстро сделал', active: false },
  { text: 'Красиво', active: false },
  { text: 'Подробно описано', active: false },
  { text: 'Все понятно и по делу', active: false }
]);

const { handleSubmit } = useForm();

const onSubmit = handleSubmit(() => {
  showError.value = false;
  // Логика отправки формы
}, () => {
  showError.value = true;
});

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
  <form class="feedback-form" @submit="onSubmit">
    <h2 class="feedback-form__title">Форма обратной связи</h2>
    <span class="feedback-form__subtitle">Пожалуйста, оцените свой опыт прохождения тестового</span>
      <div class="rating-section">
      <StarRating 
        :modelValue="rating" 
        @update:modelValue="handleRatingUpdate" 
      />
      
      <OptionButtonGroup 
        v-show="rating > 0"
        :options="options"
        @update:options="handleOptionsUpdate"
      />
    </div>
    
    <div class="form-section">
      <div class="form-row">
        <FullNameInput name="fullName" />
        <EmailInput name="email" />
      </div>
      
      <div class="form-row">
        <PhoneInput name="phone" />
        <GradeSelect name="grade" />
      </div>
    </div>

  

    <div v-if="showError" class="error-message">
      Пожалуйста, заполните все обязательные поля корректно
    </div>

    <button type="submit" class="submit-button">Отправить</button>
  </form>
</template>

<style scoped>
.feedback-form {
  max-width: 637px;
  margin: 0 auto;
  padding: 20px;
  background-color: #FFFFFF;
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

.form-section {
  margin-bottom: 30px;
}

.form-row {
  display: flex;
  gap: 20px;
  margin-bottom: 20px;
}

.rating-section {
  margin: 30px 0;
}

.error-message {
  color: #ff4444;
  background-color: #ffeeee;
  padding: 10px;
  border-radius: 4px;
  margin-bottom: 20px;
}

.submit-button {
  margin-top: 20px;
  padding: 10px 20px;
  background-color: #2196f3;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
  width: 100%;
}

.submit-button:hover {
  background-color: #0d8bf2;
}
</style>