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

const selectedGrade = ref('');
const isSelectOpen = ref(false);
const isSelectFocused = ref(false);

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
  isSelectOpen.value = false;
});

const handleMouseDown = () => {
  isSelectOpen.value = true;
};

const handleFocus = () => {
  isSelectFocused.value = true;
};

const handleBlur = () => {
  isSelectOpen.value = false;
  isSelectFocused.value = false;
};
</script>

<template>
  <div class="form-group">
    <label :for="name">Грейд</label>
    <div class="select-wrapper">
      <select
        v-model="selectedGrade"
        class="form-input"
        :class="{ 
          'form-input--error': errorMessage,
          'form-input--selected': selectedGrade,
          'form-input--focused': isSelectFocused
        }"
        :id="name"
        @mousedown="handleMouseDown"
        @focus="handleFocus"
        @blur="handleBlur"
        required
      >
        <option value="" disabled selected>Выберите</option>
        <option v-for="grade in grades" :key="grade.value" :value="grade.value">
          {{ grade.label }}
        </option>
      </select>
      <svg 
        class="select-arrow" 
        :class="{ 
          'select-arrow--open': isSelectOpen,
          'select-arrow--selected': selectedGrade,
          'select-arrow--focused': isSelectFocused
        }" 
        width="12" 
        height="8" 
        viewBox="0 0 12 8" 
        fill="none" 
        xmlns="http://www.w3.org/2000/svg"
      >
        <path d="M1 1L6 6L11 1" stroke="#A0A3BD" stroke-width="2" stroke-linecap="round"/>
      </svg>
    </div>
  </div>
</template>

<style scoped>
.form-group {
  flex: 1;
  display: flex;
  flex-direction: column;
  position: relative;
}

.select-wrapper {
  position: relative;
  max-width: 260px;
}

.form-input {
  width: 100%;
  height: 56px;
  padding: 14px 16px;
  border: 2px solid #F7F7FB;
  border-radius: 8px;
  font-size: 16px;
  line-height: 28px;
  font-weight: 400;
  background-color: #F7F7FB;
  transition: all 0.3s ease;
  appearance: none;
  -webkit-appearance: none;
  -moz-appearance: none;
  padding-right: 40px;
  color: #A0A3BD; 
  cursor: pointer;
}

.form-input:focus {
  outline: none;
  border-color: #4A3AFF;
}

.form-input--selected {
  color: #170F49;
  border-color: #F7F7FB;
}

.form-input--focused {
  border-color: #4A3AFF;
}

.form-input--selected.form-input--focused {
  border-color: #4A3AFF;
}

.form-input:invalid {
  color: #A0A3BD;
}

.form-input option {
  color: #170F49;
  background-color: white;
}

.form-input option[value=""][disabled] {
  color: #A0A3BD;
  display: block;
}

.form-input--error {
  border-color: #ff4444;
  animation: shake 0.5s ease-in-out;
}

.select-arrow {
  position: absolute;
  right: 16px;
  top: 50%;
  transform: translateY(-50%);
  pointer-events: none;
  transition: all 0.3s ease;
}

.select-arrow--open {
  transform: translateY(-50%) rotate(180deg);
}

.select-arrow--selected path {
  stroke: #4A3AFF;
}

.select-arrow--focused path {
  stroke: #4A3AFF;
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