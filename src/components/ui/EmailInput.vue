<script setup lang="ts">
import { useField } from 'vee-validate';
import { ref, watch } from 'vue';

const props = defineProps({
  name: {
    type: String,
    required: true
  }
});

const { value, errorMessage, meta } = useField<string>(props.name, (value) => {
  if (!value) return 'Email обязателен';
  if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(value)) return 'Введите корректный email';
  return true;
}, {
  validateOnValueUpdate: false 
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
      type="text" 
      class="form-input"
      :class="{ 'form-input--error': errorMessage && meta.touched }"
      :id="name"
      placeholder="example@domain.com"
    />
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
  transition: all 0.3s ease;
}

.form-input--error {
  border-color: #ff4444;
  animation: shake 0.5s ease-in-out;
}

label {
  margin-bottom: 5px;
  font-size: 14px;
  color: #333;
}

@keyframes shake {
  0%, 100% { transform: translateX(0); }
  20%, 60% { transform: translateX(-5px); }
  40%, 80% { transform: translateX(5px); }
}
</style>
