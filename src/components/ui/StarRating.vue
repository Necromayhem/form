<script setup lang="ts">
defineProps<{
  modelValue: number;
}>();

const emit = defineEmits(['update:modelValue']);

const handleClick = (rating: number) => {
  emit('update:modelValue', rating);
};
</script>

<template>
  <div class="rating">
    <div class="rating__items">
      <input
        v-for="star in 5"
        :key="star"
        class="rating__item"
        :id="`rating_${star}`"
        name="rating"
        type="radio"
        :value="star"
        :checked="modelValue === star"
        @click="handleClick(star)"
      />
      <label
        v-for="star in 5"
        :key="`label_${star}`"
        class="rating__label"
        :for="`rating_${star}`"
        :class="{ 'rating__label--active': star <= modelValue }"
      >
        ★
      </label>
    </div>
  </div>
</template>

<style scoped>
.rating {
  display: flex;
  margin-bottom: 20px;
}

.rating__item {
  display: none;
}

.rating__label {
  font-size: 24px;
  color: #ccc;
  cursor: pointer;
  transition: color 0.2s;
}

.rating__label--active {
  color: gold;
}
</style>