<script setup lang="ts">
import { ref, computed, watch } from "vue";
import { useForm } from "vee-validate";
import StarRating from "./ui/StarRating.vue";
import OptionButtonGroup from "./ui/OptionButtonGroup.vue";
import FullNameInput from "./ui/FullNameInput.vue";
import EmailInput from "./ui/EmailInput.vue";
import PhoneInput from "./ui/PhoneInput.vue";
import GradeSelect from "./ui/GradeSelect.vue";
import TextareaInput from "./ui/TextareaInput.vue";
import ErrorToaster from "./ui/ErrorToaster.vue";
import FormButtons from "./ui/FormButtons.vue";

const emit = defineEmits(["submit", "cancel"]);

const rating = ref(0);
const toaster = ref<InstanceType<typeof ErrorToaster> | null>(null);
const currentOptions = ref<Array<{ text: string; active: boolean }>>([]);

const positiveOptions = [
  { text: "Интересно", active: false },
  { text: "Легко", active: false },
  { text: "Быстро сделал", active: false },
  { text: "Красиво", active: false },
  { text: "Подробно описано", active: false },
  { text: "Все понятно и по делу", active: false },
];

const negativeOptions = [
  { text: "Не понятно", active: false },
  { text: "Однообразно", active: false },
  { text: "Скучно", active: false },
  { text: "Неудобно", active: false },
  { text: "Ничего не понял", active: false },
  { text: "Слишком сложно для тестового", active: false },
];

watch(
  rating,
  (newRating) => {
    if (newRating >= 3) {
      currentOptions.value = JSON.parse(JSON.stringify(positiveOptions));
    } else if (newRating > 0) {
      currentOptions.value = JSON.parse(JSON.stringify(negativeOptions));
    } else {
      currentOptions.value = [];
    }
  },
  { immediate: true }
);

const { handleSubmit, resetForm } = useForm();

const onSubmit = handleSubmit(
  () => {
    emit("submit");
    resetForm();
    rating.value = 0;
  },
  (context) => {
    const { errors } = context;

    if (!errors) return;
    const errorMessages = Object.values(errors)
      .flatMap((error) => {
        if (!error) return [];
        if (typeof error === "string") return error;
        return Object.values(error).flat();
      })
      .filter((error): error is string => typeof error === "string");

    if (errorMessages.length > 0) {
      toaster.value?.showError(errorMessages);
    }
  }
);

const handleRatingUpdate = (value: number) => {
  if (rating.value === value) {
    rating.value = 0;
  } else {
    rating.value = value;
  }
};

const handleOptionsUpdate = (newOptions: typeof currentOptions.value) => {
  currentOptions.value = newOptions;
};

const handleCancel = () => {
  emit("cancel");
};
</script>

<template>
  <teleport to="body">
    <form class="feedback-form" @submit.prevent="onSubmit" novalidate>
      <h2 class="feedback-form__title">Форма обратной связи</h2>
      <span class="feedback-form__subtitle"
        >Пожалуйста, оцените свой опыт прохождения тестового</span
      >

      <div class="rating-section">
        <StarRating
          :modelValue="rating"
          @update:modelValue="handleRatingUpdate"
        />

        <OptionButtonGroup
          v-show="rating > 0"
          :options="currentOptions"
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
  position: absolute;
  left: 0;
  right: 0;
  width: calc(100% - 24px);
  max-width: 637px;
  min-width: 297px;
  margin: 68px auto 0;
  padding: 44px;
  background-color: #ffffff;
  border-radius: 32px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.feedback-form__title {
  text-align: center;
  margin-bottom: 8px;
  font-size: 34px;
  line-height: 46px;
  font-weight: bold;
  color: #1f2937;
}

.feedback-form__subtitle {
  text-align: center;
  margin-bottom: 20px;
  display: block;
  font-size: 14px;
  line-height: 20px;
  font-weight: 400;
  color: #4b5563;
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

@media (max-width: 555px) {
  .form-row {
    flex-direction: column;
  }

  :deep(.feedback-form__title) {
    font-size: 18px;
    line-height: 24px;
  }

  :deep(.feedback-form__subtitle) {
    font-size: 12px;
    line-height: 16px;
  }

  :deep(.form-group) {
    justify-content: center;
  }
}

@media (max-width: 320px) {
  .feedback-form {
    padding: 20px;
  }

  :deep(.form-input) {
    width: 265px;
    height: 40px;
    font-size: 14px;
    line-height: 20px;
  }

  :deep(.select-wrapper .form-input){
    font-size: 14px;
    padding-left: 16px;
    padding-top: 8px;
    padding-bottom: 12px;
  }

  :deep(.form-group label) {
    font-size: 14px;
    line-height: 20px;
  }
}
</style>
