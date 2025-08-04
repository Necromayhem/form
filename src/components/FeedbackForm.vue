<script setup lang="ts">
import { ref } from 'vue';
import { useForm } from 'vee-validate';
import StarRating from './ui/StarRating.vue';
import OptionButtonGroup from './ui/OptionButtonGroup.vue';
import FullNameInput from './ui/FullNameInput.vue';
import EmailInput from './ui/EmailInput.vue';
import PhoneInput from './ui/PhoneInput.vue';
import GradeSelect from './ui/GradeSelect.vue';
import TextareaInput from './ui/TextareaInput.vue';
import ErrorToaster from './ui/ErrorToaster.vue';
import FormButtons from './ui/FormButtons.vue';

const emit = defineEmits(['submit', 'cancel']);

const rating = ref(0);
const toaster = ref<InstanceType<typeof ErrorToaster> | null>(null);

const options = ref([
  { text: 'Интересно', active: false },
  { text: 'Легко', active: false },
  { text: 'Быстро сделал', active: false },
  { text: 'Красиво', active: false },
  { text: 'Подробно описано', active: false },
  { text: 'Все понятно и по делу', active: false }
]);

const { handleSubmit, resetForm } = useForm();

const onSubmit = handleSubmit(() => {
  emit('submit');
  resetForm();
  rating.value = 0;
  options.value.forEach(option => option.active = false);
}, (context) => {
  const { errors } = context;
  
  if (!errors) return;
  const errorMessages = Object.values(errors)
    .flatMap(error => {
      if (!error) return [];
      if (typeof error === 'string') return error;
      return Object.values(error).flat();
    })
    .filter((error): error is string => typeof error === 'string');
  
  if (errorMessages.length > 0) {
    toaster.value?.showError(errorMessages); 
  }
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

const handleCancel = () => {
  emit('cancel');
};
</script>

<template>
  <teleport to='body'>
    <form class="feedback-form" @submit.prevent="onSubmit" novalidate>
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

        <TextareaInput name="additionalInfo" />
      </div>

      <FormButtons @cancel="handleCancel" />
    </form>
  </teleport>

  <ErrorToaster ref="toaster" />
</template>

<style scoped>
.feedback-form {
  max-width: 637px;
  margin: 0 auto;
  margin-top: 68px;
  padding: 44px;
  background-color: #FFFFFF;
  border-radius: 32px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.feedback-form__title {
  text-align: center;
  margin-bottom: 8px;
  font-size: 34px;
  line-height: 46px;
  font-weight: bold;
  color: #1F2937;
}

.feedback-form__subtitle {
  text-align: center;
  margin-bottom: 20px;
  display: block;
  font-size: 14px;
  line-height: 20px;
  font-weight: 400;
  color: #4B5563;
}

.form-section {
  margin-bottom: 30px;
}

.form-row {
  display: flex;
  gap: 20px;
  margin-bottom: 28px;
}

.rating-section {
  margin: 0 auto;
}

.submit-button {
  margin-top: 20px;
  padding: 12px 24px;
  background-color: #2196f3;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-size: 16px;
  font-weight: 500;
  width: 100%;
  transition: background-color 0.2s;
}

.submit-button:hover {
  background-color: #0d8bf2;
}

.submit-button:active {
  background-color: #0b7ad1;
}

@media (max-width: 768px) {
  .feedback-form {
    padding: 20px;
  }
  
  .form-row {
    flex-direction: column;
    gap: 15px;
  }
}
</style>
