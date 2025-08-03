<script setup lang="ts">
defineProps<{
  modelValue: number;
}>();

const emit = defineEmits(["update:modelValue"]);

const handleClick = (rating: number) => {
  emit("update:modelValue", rating);
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
      >
        <svg 
          class="rating__star" 
          width="33" 
          height="33" 
          viewBox="0 0 35 36"
        >
          <path 
            :class="{ 'rating__star-path--active': star <= modelValue }"
            class="rating__star-path"
            d="M19.0657 2.41666C18.9097 2.13845 18.6825 1.90682 18.4073 1.74559C18.1321 1.58437 17.8189 1.49939 17.5 1.49939C17.1811 1.49939 16.8679 1.58437 16.5927 1.74559C16.3175 1.90682 16.0903 2.13845 15.9343 2.41666C15.5677 3.08399 15.2151 3.75988 14.8765 4.44433C13.8099 6.60941 12.903 8.84961 12.1632 11.147C11.9083 11.9372 11.153 12.4853 10.2877 12.5092C7.66457 12.5781 5.05 12.8385 2.46484 13.2883C1.04584 13.5377 0.512335 15.1547 1.52067 16.141C1.75045 16.3671 1.98267 16.5902 2.21734 16.8102C3.97195 18.4672 5.82714 20.0144 7.77234 21.443C8.44517 21.9362 8.722 22.7813 8.46534 23.555C7.4876 26.4939 6.78787 29.5181 6.37534 32.5878C6.192 33.972 7.67334 34.9492 8.96034 34.2892C11.5124 32.9806 13.9528 31.4648 16.257 29.7572C16.6179 29.4937 17.0532 29.3518 17.5 29.3518C17.9468 29.3518 18.3821 29.4937 18.743 29.7572C21.0467 31.4655 23.4872 32.9813 26.0397 34.2892C27.3248 34.9492 28.808 33.972 28.6247 32.5878C28.5538 32.0684 28.4762 31.552 28.3918 31.0387C27.9683 28.4996 27.3474 25.9975 26.5347 23.555C26.278 22.7813 26.553 21.9362 27.2277 21.443C29.4345 19.826 31.5237 18.0542 33.4793 16.141C34.4877 15.1547 33.956 13.5377 32.5352 13.2883C29.9504 12.8359 27.3356 12.5755 24.7123 12.5092C24.2976 12.5026 23.895 12.3682 23.5593 12.1245C23.2237 11.8807 22.9714 11.5394 22.8368 11.147C21.8628 8.12322 20.5999 5.19841 19.0657 2.41666Z"
          />
        </svg>
      </label>
    </div>
  </div>
</template>

<style scoped>
.rating {
  display: flex;
  justify-content: center;
  margin: 0 auto;
  margin-bottom: 36px;
}

.rating__items {
  display: flex;
  gap: 22px;
  align-items: center;
}

.rating__item {
  display: none;
}

.rating__label {
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
}

.rating__star {
  transition: transform 0.2s ease;
}

.rating__star:hover {
  transform: scale(1.1);
}

.rating__star-path {
  fill: #FFFFFF; 
  stroke: #D9DBE9; 
  stroke-width: 1.5; 
  transition: all 0.2s ease;
}

.rating__star-path--active {
  fill: #FFE01A; 
  stroke: #FFE01A; 
}
</style>