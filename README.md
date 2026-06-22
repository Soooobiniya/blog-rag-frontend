# 📝 Blog RAG Front — 내 블로그 AI 비서 프론트엔드

> [blog-rag 백엔드](https://github.com/your-github/blog-rag) 와 연동되는 채팅 UI

---

## 📌 개요

내 네이버 블로그 글을 기반으로 자연어 질문에 답변해주는 **AI 비서 채팅 인터페이스**입니다.

---

## 🛠️ 기술 스택

| 기술 | 용도 |
|------|------|
| Vue 3 | 프론트엔드 프레임워크 |
| TypeScript | 타입 안정성 |
| Vite | 빌드 도구 |
| Axios | REST API 통신 |
| Yarn | 패키지 매니저 |

---

## 🗂️ 컴포넌트 구조

```
src/
├── App.vue                  # 루트 컴포넌트
├── views/
│   └── ChatView.vue         # 채팅 메인 화면 (상태 관리 · API 통신)
└── components/
    ├── ChatMessage.vue      # 메시지 말풍선 (user / assistant)
    └── ChatInput.vue        # 입력창 · 전송 버튼
```

---

## 💬 주요 기능

- 사용자 / AI 말풍선 구분 채팅 UI
- 엔터키 전송 지원
- 답변 생성 중 로딩 상태 표시
- 답변 근거 블로그 출처 링크 제공

---

## 🚀 실행 방법

### 사전 준비
백엔드 서버가 `http://localhost:8080` 에서 실행 중이어야 합니다.
→ [blog-rag 백엔드 실행 방법](https://github.com/your-github/blog-rag)

### 실행

```bash
# 패키지 설치
yarn

# 개발 서버 실행
yarn dev
```

브라우저에서 `http://localhost:5173` 접속

---

## 🖥️ 화면

| 초기 화면 | 대화 화면 |
|-----------|-----------|
| 블로그에 대해 무엇이든 물어보세요! | 질문 · 답변 · 출처 링크 표시 |
