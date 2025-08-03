<script setup lang="ts">
import { ref } from 'vue';

interface ToastMessage {
  id: number;
  message: string;
  severity: 'error' | 'warn' | 'info' | 'success';
  life: number;
}

const messages = ref<ToastMessage[]>([]);
let idCounter = 0;

const showError = (error: string | { [key: string]: any }) => {
  const messageText = typeof error === 'string' ? error : 
                     error.message || Object.values(error)[0] || 'Произошла ошибка';
  
  const newMessage: ToastMessage = {
    id: idCounter++,
    message: messageText,
    severity: 'error',
    life: 3000 
  };

  messages.value.push(newMessage);
  
  setTimeout(() => {
    removeMessage(newMessage.id);
  }, newMessage.life);
};

const removeMessage = (id: number) => {
  messages.value = messages.value.filter(m => m.id !== id);
};

defineExpose({ showError });
</script>

<template>
  <teleport to="body">
    <div class="toast-container">
      <transition-group name="toast-message" tag="div">
        <div 
          v-for="msg in messages" 
          :key="msg.id"
          class="toast-message"
          @click="removeMessage(msg.id)"
        >
          <div class="toast-content">
            <span class="toast-icon">!</span>
            <div class="toast-text">
              <div class="toast-title">Ошибка</div>
              <div class="toast-message-text">{{ msg.message }}</div>
            </div>
            <button class="toast-close" @click.stop="removeMessage(msg.id)">
              <span>×</span>
            </button>
          </div>
        </div>
      </transition-group>
    </div>
  </teleport>
</template>

<style scoped>
.toast-container {
  position: fixed;
  width: 350px;
  bottom: 20px;
  right: 20px;
  z-index: 1000;
}

.toast-message {
  margin-bottom: 1rem;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  background-color: white;
  border-radius: 4px;
  border-left: 4px solid #ff4444;
  position: relative;
  overflow: hidden;
  transition: all 0.3s ease;
}

.toast-content {
  display: flex;
  align-items: flex-start;
  padding: 12px 16px;
}

.toast-icon {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 20px;
  height: 20px;
  background-color: #ff4444;
  color: white;
  border-radius: 50%;
  font-weight: bold;
  margin-right: 12px;
  flex-shrink: 0;
}

.toast-text {
  flex-grow: 1;
}

.toast-title {
  font-weight: bold;
  color: #ff4444;
  margin-bottom: 4px;
}

.toast-message-text {
  color: #333;
  font-size: 14px;
}

.toast-close {
  background: none;
  border: none;
  color: #999;
  font-size: 16px;
  cursor: pointer;
  margin-left: 12px;
  padding: 0;
}

.toast-close:hover {
  color: #666;
}

.toast-message-enter-from {
  opacity: 0;
  transform: translateX(30px);
}

.toast-message-enter-active {
  transition: all 0.3s ease;
}

.toast-message-leave-active {
  transition: all 0.3s ease;
  position: absolute;
  right: 0;
  left: 0;
}

.toast-message-leave-to {
  opacity: 0;
  transform: translateY(30px);
}

.toast-message-move {
  transition: transform 0.3s;
}
</style>