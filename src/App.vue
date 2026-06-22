<template>
  <div class="app-wrapper">
    <div class="bg-blob blob-1" />
    <div class="bg-blob blob-2" />
    <div class="bg-blob blob-3" />
    <ChatView />
  </div>
</template>

<script setup lang="ts">
import ChatView from '@/views/ChatView.vue'
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: 'Pretendard', -apple-system, sans-serif;
  background: linear-gradient(135deg, #e8fdf5 0%, #f0fef4 40%, #e0fbff 100%);
  min-height: 100vh;
}

.app-wrapper {
  position: relative;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
}

.bg-blob {
  position: absolute;
  border-radius: 50%;
  filter: blur(60px);
  opacity: 0.2;
  pointer-events: none;
}

.blob-1 {
  width: 300px;
  height: 300px;
  background: #ABF15F;
  top: -80px;
  right: 100px;
}

.blob-2 {
  width: 250px;
  height: 250px;
  background: #63f2ff;
  bottom: -60px;
  right: 30px;
}

.blob-3 {
  width: 200px;
  height: 200px;
  background: #ABF15F;
  bottom: 50px;
  left: 250px;
}
</style>