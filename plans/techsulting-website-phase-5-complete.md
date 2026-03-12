## Phase 5 Complete: AI Chatbot Widget

Implemented a fully secured AI chatbot widget using the LangDock API (OpenAI-compatible) with OWASP LLM Top 10 protections (Prompt Injection detection, Output Sanitization, PII Redaction), session limits, and a DaisyUI-compliant floating chat UI integrated into BaseLayout.

**Files created/changed:**
- src/lib/security/chatbot-security.ts
- src/lib/services/langdock.ts
- src/pages/api/chatbot.ts
- src/components/chat/ChatbotWidget.astro
- src/layouts/BaseLayout.astro (updated – ChatbotWidget added)
- tests/chatbot.spec.ts

**Functions created/changed:**
- checkForInjection() – LLM01 prompt injection detection (12 patterns)
- sanitizeLlmOutput() – LLM02 XSS strip + LLM06 PII redaction
- validateChatInput() – length/type validation
- sendChatMessage() – LangDock API call with bilingual system prompts
- POST /api/chatbot – rate limit → injection check → LangDock → sanitize output

**Tests created/changed:**
- tests/chatbot.spec.ts – 6 tests (toggle visible, opens/closes, has input + send, works on other pages, EN page)

**Review Status:** APPROVED

**Git Commit Message:**
feat: add AI chatbot widget with OWASP LLM security

- LangDock API integration (OpenAI-compatible, bilingual DE/EN)
- LLM01 prompt injection detection (12 attack patterns)
- LLM02 output sanitization (XSS + HTML strip)
- LLM06 PII redaction (IBAN, cards, email, phone)
- Rate limiting (10 req/min) + session limit (20 messages)
- DaisyUI floating chat widget in BaseLayout (all pages)
- 6 new Playwright tests for chatbot widget
