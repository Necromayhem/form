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
    <div class="p-toast p-component p-toast-top-right">
      <transition-group name="p-toast-message" tag="div" class="p-toast-message-container">
        <div 
          v-for="msg in messages" 
          :key="msg.id"
          class="p-toast-message p-toast-message-error"
          @click="removeMessage(msg.id)"
        >
          <div class="p-toast-message-content">
            <span class="p-toast-message-icon pi pi-times-circle"></span>
            <div class="p-toast-message-text">
              <div class="p-toast-summary">{{ msg.message }}</div>
            </div>
            <button class="p-toast-icon-close p-link">
              <span class="pi pi-times"></span>
            </button>
          </div>
        </div>
      </transition-group>
    </div>
  </teleport>
</template>
<style scoped>
.p-toast {
  position: fixed;
  width: 500px;
  bottom: 20px;  
  right: 20px;
  z-index: 1000;
}

.p-toast-message {
  margin-bottom: 1rem;
  box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
  background-color: red;
  border-radius: 6px;
  position: relative;
  display: flex;
  overflow: hidden;
}

.p-toast .p-toast-message-container {
  position: relative;
  overflow: hidden;
}

.p-toast-message-enter-from {
  opacity: 0;
  transform: translateX(30px);
}

.p-toast-message-enter-active {
  transition: all 0.3s ease;
}

.p-toast-message-leave-active {
  transition: all 0.3s ease;
  position: absolute;
  right: 0;
  left: 0;
}

.p-toast-message-leave-to {
  opacity: 0;
  transform: translateY(-50%);
}

.p-toast-message-move {
  transition: transform 0.3s;
}
</style>