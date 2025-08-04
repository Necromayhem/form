<script setup lang="ts">
const emit = defineEmits(['retry', 'close']);

const handleRetry = () => {
  emit('retry');
};

const handleClose = () => {
  emit('close');
};
</script>

<template>
  <div class="response-container error">
    <div class="response-content">
      <h2 class="response-title">Что-то пошло не так</h2>
      <p class="response-message">Попробуйте заполнить форму позже</p>
      
      <div class="response-image"></div>
      
      <div class="button-group">
        <button 
          class="response-button secondary"
          @click="handleClose"
        >
          Закрыть
        </button>
        <button 
          class="response-button primary"
          @click="handleRetry"
        >
          Попробовать снова
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.response-container {
  max-width: 637px;
  margin: 0 auto;
  margin-top: 68px;
  padding: 44px;
  background-color: #ffffff;
  border-radius: 32px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  text-align: center;
}

.response-container.error {
  background-color: #FFF5F5;
}

.response-content {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.response-title {
 font-size: 34px;
  line-height: 46px;
  font-weight: bold;
  color: #1f2937;
  margin-bottom: 8px;
}

.response-message {
  font-size: 16px;
  line-height: 24px;
  color: #4b5563;
  margin-bottom: 95px;
}

.response-image {
  width: 158px;
  height: 144px;
  background-image: url(../assets/images/error.svg);
  margin-bottom: 95px;
}

.button-group {
  display: flex;
  gap: 28px;
  width: 100%;
  justify-content: center;
}

.response-button {
  width: 260px;
  padding: 12px 24px;
  background: #4a3aff;
  border: none;
  border-radius: 8px;
  color: white;
  cursor: pointer;
  font-size: 16px;
  line-height: 28px;
  font-weight: 400;
  transition: all 0.3s ease;
}

.response-button.primary {
  background: #4a3aff;
  color: white;
}

.response-button.primary:hover {
  background: #2A1CCA;
}

.response-button.primary:active {
  background: #191086;
}

.response-button.secondary {
  background: #f7f7fb;
  color: #4A3AFF;
  border: 1px solid #4A3AFF;
}

.response-button.secondary:hover {
  background: #eff0f6;
}

.response-button.secondary:active {
  background-color: #9CA3AF;
}
</style>
