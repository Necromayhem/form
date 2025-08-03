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
  if (!value || value.length < 18) return 'Введите полный номер телефона';
  return true;
}, {
  validateOnValueUpdate: false 
});

const phone = ref(value.value || '+7 (');

watch(phone, (newValue) => {
  let formatted = newValue.replace(/\D/g, '');
  
  if (formatted.length > 1) {
    formatted = formatted.substring(1);
  }
  
  let result = '+7 (';
  
  if (formatted.length > 0) {
    result += formatted.substring(0, 3);
  }
  if (formatted.length > 3) {
    result += ') ' + formatted.substring(3, 6);
  }
  if (formatted.length > 6) {
    result += ' ' + formatted.substring(6, 8);
  }
  if (formatted.length > 8) {
    result += ' ' + formatted.substring(8, 10);
  }
  
  phone.value = result;
  value.value = result;
});
</script>

<template>
  <div class="form-group">
    <label :for="name">Номер телефона</label>
    <input
      v-model="phone"
      type="tel"
      class="form-input"
      :class="{ 'form-input--error': errorMessage && meta.touched }"
      :id="name"
      placeholder="+7 (900) 123 45 67"
      maxlength="18"
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