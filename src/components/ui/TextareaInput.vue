<script setup lang="ts">
import { useField } from 'vee-validate';
import { ref, watch } from 'vue';

const props = defineProps({
  name: {
    type: String,
    required: true
  }
});

const { value } = useField<string>(props.name, () => true);

const textareaValue = ref(value.value);

watch(textareaValue, (newVal) => {
  value.value = newVal;
});
</script>

<template>
  <div class="form-group">
    <label :for="name">Дополнительная информация</label>
    <textarea
      v-model="textareaValue"
      class="form-textarea"
      :id="name"
      placeholder="Что понравилось и не понравилось"
    ></textarea>
  </div>
</template>

<style scoped>
.form-group {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.form-textarea {
  max-width: 549px;
  height: 144px;
  padding: 14px 16px;
  border: 2px solid #F7F7FB;
  border-radius: 8px;
  font-size: 16px;
  line-height: 28px;
  font-weight: 400;
  background-color: #F7F7FB;
  transition: all 0.3s ease;
  font-family: inherit;
  resize: none;
}

.form-textarea::placeholder {
  color: #A0A3BD;
  opacity: 1;
}

label {
  margin-bottom: 4px;
  font-size: 14px;
  line-height: 20px;
  font-weight: 400;
  color: #6F6C90;
}
</style>