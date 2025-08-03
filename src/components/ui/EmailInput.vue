<script setup lang="ts">
import { useField } from 'vee-validate';
import { ref, watch } from 'vue';

const props = defineProps({
  name: {
    type: String,
    required: true
  }
});

const { value, errorMessage } = useField<string>(props.name, (value) => {
  if (!value) return 'Email обязателен';
  if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(value)) return 'Введите корректный email';
  return true;
});

const inputValue = ref(value.value);

watch(inputValue, (newVal) => {
  value.value = newVal;
});
</script>

<template>
  <div class="form-group">
    <label :for="name">Почта</label>
    <input
      v-model="inputValue"
      type="email"
      class="form-input"
      :id="name"
      placeholder="example@domain.com"
    />
    <span v-if="errorMessage" class="error-text">{{ errorMessage }}</span>
  </div>
</template>

<style scoped>
.form-group {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.form-input {
  padding: 10px;
  border: 1px solid #e0e0e0;
  border-radius: 4px;
  font-size: 14px;
}

label {
  margin-bottom: 5px;
  font-size: 14px;
  color: #333;
}

.error-text {
  color: #ff4444;
  font-size: 12px;
  margin-top: 5px;
}
</style>