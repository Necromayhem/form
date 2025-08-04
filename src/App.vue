<script setup lang="ts">
import { ref } from 'vue';
import ErrorResponse from './components/ErrorResponse.vue';
import FeedbackForm from './components/FeedbackForm.vue';
import SuccessResponse from './components/SuccessResponse.vue';

const showForm = ref(true);
const showSuccess = ref(false);
const showError = ref(false);

const handleFormSubmit = () => {
  showForm.value = false;
  if (Math.random() > 0.5) {
    showSuccess.value = true;
  } else {
    showError.value = true;
  }
};

const resetForm = () => {
  showForm.value = true;
  showSuccess.value = false;
  showError.value = false;
};

const closeForm = () => {
  showForm.value = false;
  showSuccess.value = false;
  showError.value = false;
};
</script>

<template>
  <FeedbackForm 
    v-if="showForm" 
    @submit="handleFormSubmit" 
    @cancel="closeForm"
  />
  <SuccessResponse 
    v-if="showSuccess" 
    @back-to-home="resetForm" 
  />
  <ErrorResponse 
    v-if="showError" 
    @retry="resetForm" 
    @close="closeForm" 
  />
</template>

<style>
</style>