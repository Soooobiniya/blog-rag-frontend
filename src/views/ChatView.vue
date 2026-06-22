<template>
  <div class="layout">
    <aside class="sidebar">
      <div class="sidebar-header">
        <div class="logo">
          <div class="logo-icon">
            <img src="/profile.jpg" alt="프로필" style="width:100%;height:100%;object-fit:cover;border-radius:8px;" />
          </div>
          <div>
            <div class="logo-title">오복잉</div>
            <div class="logo-sub">빈수 블로그 AI 비서</div>
          </div>
        </div>
      </div>

      <div class="section-label">크롤링 현황</div>
      <div class="crawl-card">
        <div class="crawl-row">
          <span class="crawl-num">{{ postingCount }}</span>
          <span class="crawl-unit">개</span>
        </div>
        <div class="crawl-label">저장된 포스팅</div>
        <button class="crawl-btn" @click="crawl" :disabled="crawling">
          <i class="ti ti-refresh" aria-hidden="true" />
          {{ crawling ? '크롤링 중...' : '새로 크롤링' }}
        </button>
      </div>

      <div class="section-label">대화 기록</div>
      <div class="chat-list">
        <div v-for="(session, i) in sessions" :key="i" :class="['chat-item', { active: currentSession === i }]"
          @click="currentSession = i">
          <i class="ti ti-message" aria-hidden="true" />
          {{ session.title }}
        </div>
      </div>

      <button class="new-btn" @click="newSession">
        <i class="ti ti-plus" aria-hidden="true" /> 새 대화 시작
      </button>
    </aside>

    <main class="main">
      <div class="main-header">
        <div class="header-title">
          {{ sessions[currentSession]?.title ?? '새 대화' }}
        </div>
      </div>

      <div class="chat-area" ref="chatArea">
        <div v-if="messages.length === 0" class="empty">
          블로그에 대해 무엇이든 물어보세요 🌿
        </div>
        <ChatMessage v-for="(msg, i) in messages" :key="i" :role="msg.role" :content="msg.content"
          :sources="msg.sources" />
        <div v-if="loading" class="msg assistant">
          <div class="assistant-name">
            <i class="ti ti-sparkles" aria-hidden="true" /> AI 비서
          </div>
          <div class="typing">
            <div class="dot" />
            <div class="dot" />
            <div class="dot" />
          </div>
        </div>
      </div>

      <ChatInput :loading="loading" @send="sendMessage" />
    </main>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, nextTick } from 'vue'
import axios from 'axios'
import ChatMessage from '@/components/ChatMessage.vue'
import ChatInput from '@/components/ChatInput.vue'

interface Message {
  role: 'user' | 'assistant'
  content: string
  sources?: string[]
}

interface Session {
  title: string
  messages: Message[]
}

const sessions = ref<Session[]>([{ title: '새 대화', messages: [] }])
const currentSession = ref(0)
const loading = ref(false)
const crawling = ref(false)
const postingCount = ref(0)
const chatArea = ref<HTMLElement | null>(null)

const messages = ref<Message[]>(sessions.value[0].messages)

const scrollToBottom = async () => {
  await nextTick()
  if (chatArea.value) {
    chatArea.value.scrollTop = chatArea.value.scrollHeight
  }
}

const fetchStatus = async () => {
  const res = await axios.get('http://localhost:8080/api/crawl/status')
  postingCount.value = res.data.count
}

const crawl = async () => {
  crawling.value = true
  await axios.post('http://localhost:8080/api/crawl')
  await fetchStatus()
  crawling.value = false
}

const newSession = () => {
  sessions.value.push({ title: '새 대화', messages: [] })
  currentSession.value = sessions.value.length - 1
  messages.value = sessions.value[currentSession.value].messages
}

const sendMessage = async (message: string) => {
  messages.value.push({ role: 'user', content: message })

  if (messages.value.length === 1) {
    sessions.value[currentSession.value].title = message.slice(0, 20)
  }

  await scrollToBottom()
  loading.value = true

  try {
    const res = await axios.post('http://localhost:8080/api/chat', { message })
    messages.value.push({
      role: 'assistant',
      content: res.data.answer,
      sources: res.data.sources
    })
  } catch {
    messages.value.push({ role: 'assistant', content: '오류가 발생했어요. 다시 시도해줘요.' })
  } finally {
    loading.value = false
    await scrollToBottom()
  }
}

onMounted(fetchStatus)
</script>

<style scoped>
.layout {
  display: flex;
  height: 100vh;
  backdrop-filter: blur(2px);
}

.sidebar {
  width: 230px;
  display: flex;
  flex-direction: column;
  background: rgba(255, 255, 255, 0.55);
  border-right: 1px solid rgba(99, 242, 255, 0.25);
  flex-shrink: 0;
}

.sidebar-header {
  padding: 18px 16px 14px;
  border-bottom: 1px solid rgba(99, 242, 255, 0.2);
}

.logo {
  display: flex;
  align-items: center;
  gap: 10px;
}

.logo-icon {
  width: 30px;
  height: 30px;
  border-radius: 8px;
  background: linear-gradient(135deg, #63f2ff, #abf15f);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 15px;
  color: #1a3a2a;
}

.logo-title {
  font-size: 15px;
  font-weight: 500;
  color: #1a3a3a;
}

.logo-sub {
  font-size: 11px;
  color: #63c4cc;
}

.section-label {
  padding: 12px 16px 6px;
  font-size: 10px;
  font-weight: 500;
  color: #7abfc4;
  letter-spacing: 0.06em;
  text-transform: uppercase;
}

.crawl-card {
  margin: 0 12px;
  background: rgba(255, 255, 255, 0.7);
  border: 1px solid rgba(99, 242, 255, 0.3);
  border-radius: 14px;
  padding: 12px;
}

.crawl-row {
  display: flex;
  align-items: baseline;
  gap: 4px;
}

.crawl-num {
  font-size: 26px;
  font-weight: 500;
  color: #abf15f;
  line-height: 1;
}

.crawl-unit {
  font-size: 12px;
  color: #7abfc4;
}

.crawl-label {
  font-size: 11px;
  color: #7abfc4;
  margin-top: 2px;
}

.crawl-btn {
  margin-top: 10px;
  width: 100%;
  padding: 7px;
  border-radius: 10px;
  border: 1.5px solid #63f2ff;
  background: rgba(99, 242, 255, 0.08);
  color: #2ab8c8;
  font-size: 12px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 5px;
}

.crawl-btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.chat-list {
  flex: 1;
  overflow-y: auto;
  padding: 6px 8px;
}

.chat-item {
  padding: 8px 10px;
  border-radius: 10px;
  font-size: 12px;
  color: #5a8a90;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 7px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  margin-bottom: 2px;
}

.chat-item.active {
  background: rgba(255, 255, 255, 0.75);
  color: #1a4a4a;
  border: 1px solid rgba(99, 242, 255, 0.25);
}

.new-btn {
  margin: 8px;
  padding: 8px 12px;
  border-radius: 10px;
  border: 1px dashed rgba(99, 242, 255, 0.4);
  background: transparent;
  font-size: 12px;
  color: #7abfc4;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 6px;
}

.main {
  flex: 1;
  display: flex;
  flex-direction: column;
  min-width: 0;
}

.main-header {
  padding: 14px 20px;
  border-bottom: 1px solid rgba(99, 242, 255, 0.2);
  background: rgba(255, 255, 255, 0.4);
}

.header-title {
  font-size: 14px;
  font-weight: 500;
  color: #1a4a4a;
  display: flex;
  align-items: center;
  gap: 8px;
}

.header-title::before {
  content: '';
  display: inline-block;
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: #abf15f;
}

.chat-area {
  flex: 1;
  overflow-y: auto;
  padding: 20px;
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.empty {
  text-align: center;
  color: #7abfc4;
  margin: auto;
  font-size: 14px;
}

.msg {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.msg.assistant {
  align-items: flex-start;
}

.assistant-name {
  font-size: 11px;
  color: #63c4cc;
  display: flex;
  align-items: center;
  gap: 4px;
}

.typing {
  display: flex;
  align-items: center;
  gap: 5px;
  padding: 12px 16px;
  background: rgba(255, 255, 255, 0.75);
  border: 1px solid rgba(99, 242, 255, 0.3);
  border-radius: 16px;
  border-bottom-left-radius: 4px;
}

.dot {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background: #63f2ff;
  animation: bounce 1.2s infinite;
}

.dot:nth-child(2) {
  animation-delay: 0.2s;
  background: #96f4aa;
}

.dot:nth-child(3) {
  animation-delay: 0.4s;
  background: #abf15f;
}

@keyframes bounce {

  0%,
  60%,
  100% {
    transform: translateY(0);
  }

  30% {
    transform: translateY(-5px);
  }
}
</style>