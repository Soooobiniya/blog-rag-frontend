<template>
  <div class="container">
    <header>
      <h1>📝 내 블로그 AI 비서</h1>
    </header>

    <div class="chat-box">
      <div v-if="messages.length === 0" class="empty">
        블로그에 대해 무엇이든 물어보세요!
      </div>
      <ChatMessage v-for="(msg, i) in messages" :key="i" :role="msg.role" :content="msg.content"
        :sources="msg.sources" />
      <div v-if="loading" class="message assistant">
        <div class="bubble">답변 생성 중...</div>
      </div>
    </div>

    <ChatInput :loading="loading" @send="sendMessage" />
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import axios from 'axios'
import ChatMessage from '@/components/ChatMessage.vue'
import ChatInput from '@/components/ChatInput.vue'

interface Message {
  role: 'user' | 'assistant'
  content: string
  sources?: string[]
}

const messages = ref<Message[]>([])
const loading = ref(false)

const sendMessage = async (message: string) => {
  messages.value.push({ role: 'user', content: message })
  loading.value = true

  try {
    const response = await axios.post('http://localhost:8080/api/chat', { message })
    messages.value.push({
      role: 'assistant',
      content: response.data.answer,
      sources: response.data.sources
    })
  } catch {
    messages.value.push({ role: 'assistant', content: '오류가 발생했어요. 다시 시도해줘요.' })
  } finally {
    loading.value = false
  }
}
</script>

<style scoped>
.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  height: 100vh;
  display: flex;
  flex-direction: column;
}

header h1 {
  font-size: 1.5rem;
  margin-bottom: 16px;
}

.chat-box {
  flex: 1;
  overflow-y: auto;
  border: 1px solid #e0e0e0;
  border-radius: 12px;
  padding: 16px;
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.empty {
  text-align: center;
  color: #aaa;
  margin: auto;
}

.message.assistant {
  align-items: flex-start;
  display: flex;
  flex-direction: column;
}

.bubble {
  padding: 10px 14px;
  border-radius: 12px;
  background: #f3f4f6;
  color: #111;
  line-height: 1.5;
}
</style>