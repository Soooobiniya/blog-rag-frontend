<template>
  <div :class="['message', role]">
    <div class="bubble">{{ content }}</div>
    <div v-if="sources?.length" class="sources">
      <a v-for="(src, i) in sources" :key="i" :href="src" target="_blank">
        📎 출처 {{ i + 1 }}
      </a>
    </div>
  </div>
</template>

<script setup lang="ts">
defineProps<{
  role: 'user' | 'assistant'
  content: string
  sources?: string[]
}>()
</script>

<style scoped>
.message {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.message.user {
  align-items: flex-end;
}

.message.assistant {
  align-items: flex-start;
}

.bubble {
  max-width: 70%;
  padding: 10px 14px;
  border-radius: 12px;
  line-height: 1.5;
  white-space: pre-wrap;
}

.message.user .bubble {
  background: #4f46e5;
  color: white;
}

.message.assistant .bubble {
  background: #f3f4f6;
  color: #111;
}

.sources {
  display: flex;
  gap: 8px;
  flex-wrap: wrap;
}

.sources a {
  font-size: 0.8rem;
  color: #4f46e5;
  text-decoration: none;
}
</style>