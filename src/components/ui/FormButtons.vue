<script setup lang="ts"></script>

<template>
  <div class="buttons-container">
    <button type="button" class="cancel-button">Отменить</button>
    <button type="submit" class="submit-button">Отправить</button>
  </div>
</template>

<style scoped>
.buttons-container {
  display: flex;
  max-width: 549px;
  height: 56px;
  gap: 28px;
  margin-top: 40px;
}

.cancel-button {
  width: 260px;
  padding: 12px 24px;
  background: #f7f7fb;
  border: 1px solid #d9dbe9;
  border-radius: 8px;
  color: #4A3AFF;
  cursor: pointer;
  font-size: 16px;
  line-height: 28px;
  font-weight: 400;
  transition: all 0.3s ease;
}

.cancel-button:hover {
  background: #eff0f6;
}

.cancel-button:active {
  background-color: #9CA3AF;
}

.submit-button {
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

.submit-button:hover {
  background: #2A1CCA;
}

.submit-button:active {
  background: #191086;
}
</style>
