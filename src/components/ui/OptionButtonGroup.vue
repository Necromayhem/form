<script setup lang="ts">
import { ref } from 'vue';

interface Option {
  text: string;
  active: boolean;
}

const props = defineProps<{
  options: Option[];
}>();

const emit = defineEmits(['update:options']);

const toggleOption = (index: number) => {
  const newOptions = [...props.options];
  newOptions[index].active = !newOptions[index].active;
  emit('update:options', newOptions);
};
</script>

<template>
  <div class="options">
    <div class="options__title">Что вам понравилось?</div>
    <div class="options__list">
      <button
        v-for="(option, index) in options"
        :key="option.text"
        type="button"
        class="options__item"
        :class="{ 'options__item--active': option.active }"
        @click="toggleOption(index)"
      >
        {{ option.text }}
      </button>
    </div>
  </div>
</template>

<style scoped>
.options {
  margin-top: 20px;
}

.options__title {
  margin-bottom: 10px;
  font-weight: bold;
}

.options__list {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
}

.options__item {
  padding: 8px 16px;
  border: 1px solid #e0e0e0;
  border-radius: 20px;
  background: #f5f5f5;
  cursor: pointer;
  transition: all 0.2s ease;
  font-size: 14px;
}

.options__item:hover {
  background: #e0e0e0;
  border-color: #bdbdbd;
}

.options__item--active {
  background: #bbdefb;
  border-color: #2196f3;
  color: #0d47a1;
}

.options__item--active:hover {
  background: #90caf9;
}
</style>