<script setup lang="ts">
import { ref } from 'vue';

interface ToastMessage {
  id: number;
  messages: string[];
  severity: 'error' | 'warn' | 'info' | 'success';
  life: number;
}

const messages = ref<ToastMessage[]>([]);
let idCounter = 0;
let timeoutId: ReturnType<typeof setTimeout> | null = null;

const showError = (errors: string[] | string | { [key: string]: any }) => {
  messages.value = [];
  if (timeoutId) {
    clearTimeout(timeoutId);
    timeoutId = null;
  }
  let errorMessages: string[];
  if (Array.isArray(errors)) {
    errorMessages = errors.filter(msg => typeof msg === 'string');
  } else if (typeof errors === 'string') {
    errorMessages = [errors];
  } else {
    errorMessages = Object.values(errors)
      .flat()
      .filter((msg): msg is string => typeof msg === 'string');
  }

  if (errorMessages.length === 0) {
    errorMessages = ['Произошла ошибка'];
  }
  const newMessage: ToastMessage = {
    id: idCounter++,
    messages: errorMessages, 
    severity: 'error',
    life: 3000, 
  };

  messages.value = [newMessage]; 

  timeoutId = setTimeout(() => {
    removeMessage(newMessage.id);
    timeoutId = null;
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
              <div class="toast-title">Ошибки в форме</div>
              <ul class="toast-message-list">
                <li v-for="(error, index) in msg.messages" :key="index">
                  {{ error }}
                </li>
              </ul>
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

.toast-message-list {
  margin: 0;
  padding-left: 20px;
}

.toast-message-list li {
  margin-bottom: 4px;
  font-size: 14px;
  color: #333;
}

.toast-message-list li:last-child {
  margin-bottom: 0;
}

@media (max-width: 400px) {
  .toast-container {
    width: 280px;
    right: 10px;
    bottom: 10px;
  }

  .toast-message {
    margin-bottom: 0.5rem;
  }

  .toast-content {
    padding: 8px 12px;
  }

  .toast-title {
    font-size: 14px;
  }

  .toast-message-list li {
    font-size: 12px;
  }

  .toast-icon {
    width: 16px;
    height: 16px;
    font-size: 12px;
    margin-right: 8px;
  }

  .toast-close {
    font-size: 14px;
  }
}
</style>