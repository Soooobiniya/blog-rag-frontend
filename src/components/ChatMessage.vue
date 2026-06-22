<template>
  <div :class="['msg', role]">
    <div v-if="role === 'assistant'" class="assistant-name">
      <i class="ti ti-sparkles" aria-hidden="true"></i> AI 비서
    </div>
    <div class="bubble">{{ content }}</div>
    <div v-if="sources?.length" class="sources">
      <a v-for="(src, i) in sources" :key="i" :href="src" target="_blank" class="chip">
        <i class="ti ti-link" aria-hidden="true" style="font-size:11px"></i>
        출처 {{ i + 1 }}
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
.msg {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.msg.user {
  align-items: flex-end;
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

.bubble {
  max-width: 75%;
  padding: 10px 14px;
  font-size: 13px;
  line-height: 1.65;
  border-radius: 16px;
}

.msg.user .bubble {
  background: linear-gradient(135deg, #abf15f, #8fd94a);
  color: #1a3000;
  border-bottom-right-radius: 4px;
  border: 1.5px solid rgba(171, 241, 95, 0.5);
}

.msg.assistant .bubble {
  background: rgba(255, 255, 255, 0.75);
  color: #1a4a4a;
  border: 1px solid rgba(99, 242, 255, 0.3);
  border-bottom-left-radius: 4px;
}

.sources {
  display: flex;
  gap: 6px;
  flex-wrap: wrap;
}

.chip {
  font-size: 11px;
  color: #2ab8c8;
  background: rgba(99, 242, 255, 0.1);
  border: 1px solid rgba(99, 242, 255, 0.3);
  border-radius: 20px;
  padding: 3px 10px;
  display: flex;
  align-items: center;
  gap: 4px;
  text-decoration: none;
}
</style>