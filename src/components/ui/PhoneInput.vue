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

const phone = ref(value.value || '');

watch(phone, (newValue) => {
  if (!newValue || newValue === '+7 (') {
    phone.value = '';
    value.value = '';
    return;
  }

  let formatted = newValue.replace(/\D/g, '');
  
  if (!formatted) {
    phone.value = '';
    value.value = '';
    return;
  }
  
  if (formatted.startsWith('7')) {
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
      placeholder="+7 (000) 000 00 00"
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
  max-width: 260px;
  height: 56px;
  padding: 14px 16px;
  border: 2px solid #F7F7FB;
  border-radius: 8px;
  font-size: 16px;
  line-height: 28px;
  font-weight: 400;
  background-color: #F7F7FB;
  transition: all 0.3s ease;
}

.form-input::placeholder {
  color: #A0A3BD;
  opacity: 1;
}

.form-input--error {
  border-color: #ff4444;
  animation: shake 0.5s ease-in-out;
}

label {
  margin-bottom: 4px;
  font-size: 14px;
  line-height: 20px;
  font-weight: 400;
  color: #6F6C90;
}

@keyframes shake {
  0%, 100% { transform: translateX(0); }
  20%, 60% { transform: translateX(-5px); }
  40%, 80% { transform: translateX(5px); }
}
</style>