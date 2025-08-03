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
});
</script>

<template>
  <div class="form-group">
    <label :for="name">Грейд</label>
    <select
      v-model="selectedGrade"
      class="form-input"
      :id="name"
    >
      <option value="" disabled>Выберите</option>
      <option v-for="grade in grades" :key="grade.value" :value="grade.value">
        {{ grade.label }}
      </option>
    </select>
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
  background-color: white;
  appearance: none;
  -webkit-appearance: none;
  -moz-appearance: none;
  background-image: url("data:image/svg+xml;charset=UTF-8,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='currentColor' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'%3e%3cpolyline points='6 9 12 15 18 9'%3e%3c/polyline%3e%3c/svg%3e");
  background-repeat: no-repeat;
  background-position: right 10px center;
  background-size: 1em;
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