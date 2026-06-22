<template>
  <div class="input-area">
    <input v-model="input" @keyup.enter="send" placeholder="블로그에 대해 질문해보세요" :disabled="loading" class="input-box" />
    <button class="send-btn" @click="send" :disabled="loading">
      <i class="ti ti-arrow-up" aria-hidden="true"></i>
    </button>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const props = defineProps<{ loading: boolean }>()
const emit = defineEmits<{ send: [message: string] }>()

const input = ref('')

const send = () => {
  if (!input.value.trim() || props.loading) return
  emit('send', input.value.trim())
  input.value = ''
}
</script>

<style scoped>
.input-area {
  padding: 12px 20px 16px;
  border-top: 1px solid rgba(99, 242, 255, 0.2);
  background: rgba(255, 255, 255, 0.4);
  display: flex;
  gap: 8px;
  align-items: center;
}

.input-box {
  flex: 1;
  background: rgba(255, 255, 255, 0.7);
  border: 1.5px solid rgba(99, 242, 255, 0.35);
  border-radius: 12px;
  padding: 10px 14px;
  font-size: 13px;
  color: #1a4a4a;
  outline: none;
}

.input-box::placeholder {
  color: #7abfc4;
}

.input-box:focus {
  border-color: rgba(99, 242, 255, 0.6);
}

.send-btn {
  width: 38px;
  height: 38px;
  border-radius: 50%;
  background: linear-gradient(135deg, #63f2ff, #abf15f);
  border: none;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  flex-shrink: 0;
  font-size: 16px;
  color: #1a3a2a;
}

.send-btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}
</style>