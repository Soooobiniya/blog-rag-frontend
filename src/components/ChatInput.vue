<template>
  <div class="input-area">
    <input v-model="input" @keyup.enter="send" placeholder="블로그에 대해 질문해보세요" :disabled="loading" />
    <button @click="send" :disabled="loading">전송</button>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const emit = defineEmits<{
  send: [message: string]
}>()

const input = ref('')

const props = defineProps<{
  loading: boolean
}>()

const send = () => {
  if (!input.value.trim() || props.loading) return
  emit('send', input.value.trim())
  input.value = ''
}
</script>

<style scoped>
.input-area {
  display: flex;
  gap: 8px;
  margin-top: 16px;
}

input {
  flex: 1;
  padding: 10px 14px;
  border: 1px solid #e0e0e0;
  border-radius: 8px;
  font-size: 1rem;
  outline: none;
}

button {
  padding: 10px 20px;
  background: #4f46e5;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 1rem;
}

button:disabled {
  opacity: 0.5;
}
</style>