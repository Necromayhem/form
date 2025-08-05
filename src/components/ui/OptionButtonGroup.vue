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
  max-width: 549px;
  margin: 0 auto;
  margin-bottom: 36px;
}

.options__list {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  gap: 16px;
}

.options__item {
  padding: 5px 16px;
  border: 1px solid #e0e0e0;
  border-radius: 20px;
  background: #EFF0F6;
  cursor: pointer;
  transition: all 0.2s ease;
  font-size: 16px;
  line-height: 28px;
  font-weight: 400;
  color: #170F49;
}

.options__item--active {
  background: #6F6C90;
  color: #FFFFFF;
  
}

</style>