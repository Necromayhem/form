<script setup lang="ts">
import { useField } from 'vee-validate';
import { ref, watch } from 'vue';

const props = defineProps({
  name: {
    type: String,
    required: true
  },
  validateOnInput: {
    type: Boolean,
    default: false
  }
});

const { value, errorMessage, validate } = useField<string>(props.name, (value) => {
  if (!value) return 'Выберите грейд';
  return true;
});

const selectedGrade = ref(value.value);

const grades = [
  { value: 'junior', label: 'Junior' },
  { value: 'middle', label: 'Middle' },
  { value: 'senior', label: 'Senior' },
  { value: 'teamlead', label: 'Team Lead' }
];

watch(selectedGrade, (newValue) => {
  value.value = newValue;
  if (props.validateOnInput) {
    validate();
  }
});
</script>

<template>
  <div class="form-group">
    <label :for="name">Грейд</label>
    <select
      v-model="selectedGrade"
      class="form-input"
      :class="{ 'form-input--error': errorMessage }"
      :id="name"
    >
      <option value="" disabled>Выберите</option>
      <option v-for="grade in grades" :key="grade.value" :value="grade.value">
        {{ grade.label }}
      </option>
    </select>
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
  background-color: white;
  appearance: none;
  -webkit-appearance: none;
  -moz-appearance: none;
  background-image: url("data:image/svg+xml;charset=UTF-8,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='currentColor' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'%3e%3cpolyline points='6 9 12 15 18 9'%3e%3c/polyline%3e%3c/svg%3e");
  background-repeat: no-repeat;
  background-position: right 10px center;
  background-size: 1em;
  transition: all 0.3s ease;
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