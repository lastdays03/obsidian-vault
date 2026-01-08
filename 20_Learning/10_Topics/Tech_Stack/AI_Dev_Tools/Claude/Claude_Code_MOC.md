---
tags: [knowledge/map, tool/claude]
Up: [[Tech_Stack_MOC]]
---

# Claude Code Mastery MOC

**Claude Code CLI**는 개발자의 로컬 환경과 LLM 사이를 중재하는 차세대 에이전트 도구입니다. 이 MOC는 Apple Silicon 환경 최적화부터 엔터프라이즈급 TDD 아키텍처까지의 마스터리 가이드를 모듈별로 정리합니다.

## 📚 핵심 모듈 (Modules)

### 0. [[Claude_Code_Handbook|The Ultimate Handbook]]
- User-Verified Features (Terminal, Patching, MCP)
- Power User Features (Headless, piping, Custom Commands)

### 0. Framework & Setup
- [[AI_Dev_Tools/Agentic_Orchestration|Agentic Orchestration]]: The theoretical framework. (Advanced)
- [[Claude_Code_Setup_Guide]]: The practical configuration guide (.claude config, standards).

### 1. [[01_CLI_Foundation|CLI Foundation (기반 환경)]]
- Apple Silicon 최적화 및 설치
- React-in-Terminal 아키텍처 이해
- Thinking Mode(사고 모드) 활용 전략
- `CLAUDE.md` 템플릿 (Senior Level)

### 2. [[02_MCP_Ecosystem|MCP Ecosystem & Skills]]
- MCP(Model Context Protocol)와 Skill의 차이
- 필수 MCP 서버 (Filesystem, GitHub, Postgres 등)
- Custom Skill 제작: Architecture Validator

### 3. [[03_Orchestration_n8n|Orchestration with n8n]]
- Headless Mode 활용
- n8n을 이용한 HITL(Human-in-the-loop) 승인 프로세스
- Slack 연동 자동화

### 4. [[04_Future_Proofing|Future Proofing (미래 대응)]]
- Docs-to-Skill 파이프라인 (최신 문서 실시간 학습)
- 라이브러리 버전 정합성 검증 (Version-Agnostic Coding)

### 5. [[05_Enterprise_Architecture|Enterprise Architecture]]
- DevContainers를 이용한 Docker 샌드박싱
- Agentic TDD (Test-Driven Development) 프로토콜

---

## 🔗 관련 리소스
- [[Antigravity_MOC|Antigravity Agent Framework]]
- [[Python_MOC]]
