<script setup lang="ts">
const emit = defineEmits(["back-to-home"]);

const handleBackClick = () => {
  emit("back-to-home");
};
</script>

<template>
  <div class="response-container">
    <div class="response-content">
      <h2 class="response-title">Спасибо, ваш ответ отправлен</h2>
      <p class="response-message">Свяжемся с вами в ближайшее время</p>

      <div class="response-image"></div>

      <button class="response-button" @click="handleBackClick">
        На главную
      </button>
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
  background-image: url(../assets/images/success.svg);
  margin-bottom: 95px;
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

.response-button:hover {
  background: #2A1CCA;
}

.response-button:active {
  background: #191086;
}

@media (max-width: 650px){
  .response-container {
    margin-left: 12px;
    margin-right: 12px;
  }
}

@media (max-width: 350px) {
  .response-container {
    padding: 20px;
  }

  .response-title {
    font-size: 18px;
    line-height: 24px;
  }

  .response-message {
    font-size: 12px;
    line-height: 16px;
    margin-bottom: 40px;
  }

  .response-image {
    width: 100px;
    height: 90px;
    margin-bottom: 40px;
    background-size: contain;
    background-repeat: no-repeat;
    background-position: center;
  }

  .response-button {
    width: 100%;
    font-size: 14px;
    line-height: 20px;
    padding: 8px 16px;
  }
}
</style>