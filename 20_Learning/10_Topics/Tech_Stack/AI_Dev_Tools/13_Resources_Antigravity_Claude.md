---
tags:
  - knowledge/resource
  - tool/antigravity
  - tool/claude-code
Up: [[12_Antigravity_Multi_Agent_Strategy]]
---

# 13. Resources: Antigravity & Claude Code Integration

본 문서는 **Google Antigravity IDE**와 **Claude Code CLI**를 결합한 "Dual Core" 전략을 위한 핵심 자료 모음입니다. 
(분석 소스: `a1.md`, `a2.md`, `a3.md` 통합)

---

## 🏆 The Best Link (One-Pick)

당장 워크플로우를 셋업해야 한다면, 이 영상 하나만 보시면 됩니다.

- **Title:** **"I Tried Claude Code Inside Antigravity — It's Actually Insane"**
- **Type:** Video Tutorial
- **URL:** [https://www.youtube.com/watch?v=GaYQMOAFdds](https://www.youtube.com/watch?v=GaYQMOAFdds)
- **선정 이유:**
    - 공식 통합이 없는 상태에서 **"터미널 해킹"**을 통해 Antigravity 내에서 Claude Code를 실행하는 법을 정확히 보여줍니다.
    - Gemini Agent(기획)와 Claude CLI(코딩)가 한 화면에서 돌아가는 **"Real-Time Dual Core"** 데모가 포함되어 있습니다.

---

## 📚 Curated Resource Table (2025.11 ~ 2026.01)

커뮤니티에서 검증된 Top-tier 리소스입니다.

| Type       | Platform | Title & Key Takeaway                                                                                                      | URL                                                                            |
| :--------- | :------- | :------------------------------------------------------------------------------------------------------------------------ | :----------------------------------------------------------------------------- |
| **Video**  | YouTube  | **Adding App Auth (Antigravity + Claude)**<br>Gemini가 기획하고 Claude가 인증 로직을 짜는 하이브리드 워크플로우 데모.     | [Watch Video](https://www.youtube.com/watch?v=yMJcHcCbgi4)                     |
| **Video**  | YouTube  | **Antigravity + Claude Code Is INSANE!**<br>파일 공유와 토큰 최적화 팁이 포함된 심화 가이드.                              | [Watch Video](https://www.youtube.com/watch?v=-lByERj69UQ)                     |
| **Hack**   | GitHub   | **antigravity-claude-proxy**<br>Antigravity의 내부 모델을 Anthropic API처럼 속여서 Claude Code가 쓰게 만드는 프록시 도구. | [GitHub Repo](https://github.com/badrisnarayanan/antigravity-claude-proxy)     |
| **Review** | Reddit   | **Antigravity vs Cursor (Honest Review)**<br>에이전트 모드에서의 "환각" 이슈와 이를 Claude로 보완하는 팁 공유.            | [Reddit Thread](https://www.reddit.com/r/google_antigravity/comments/1q1tx8j/) |
| **Review** | Dev.to   | **Weightless Code (7-Day Experiment)**<br>7일간의 실험기. 파일 네비게이션을 Claude CLI로 대체하는 노하우.                 | [Read Article](https://dev.to/naresh_007/weightless-code)                      |

---

## 🛠️ Reality Mapping (2025 Real-World Equivalents)

만약 **"Antigravity"가 출시되기 전(2025년 기준)**에 이 전략을 당장 구현하고 싶다면, 다음의 **실존 도구**들로 완벽하게 대체할 수 있습니다. (`a1.md` 분석 기반)

| 역할 (Role)                            | 가상 도구 (Antigravity Scenario) | **현실 대체재 (Real World 2025)**                                                                                                                             |
| :------------------------------------- | :------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **The Basecamp**<br>(IDE & Cloud Env)  | **Google Antigravity**           | **Google Project IDX** ([idx.dev](https://idx.dev))<br>구글의 클라우드 IDE로, `nix` 환경을 통해 어떤 CLI 도구든 설치 가능합니다.                              |
| **The Staff**<br>(Autonomous Agent)    | **Antigravity Agent**            | **Cline (Open Source)** ([github.com/cline/cline](https://github.com/cline/cline))<br>VS Code 사이드바에서 터미널과 파일을 직접 제어하는 자율 에이전트입니다. |
| **The Finisher**<br>(Reasoning Engine) | **Claude Code CLI**              | **Simon Willison's `llm` CLI**<br>터미널에서 `claude` 모델을 호출하여 파이프라인(`                                                                            | `) 작업을 수행하는 도구입니다. |

### 💡 Director's Action Plan (2025 ver.)
1.  **Google Project IDX**를 엽니다.
2.  확장 프로그램으로 **Cline**을 설치하고 **Claude 3.5 Sonnet** API를 연결합니다.
3.  터미널에 `llm` 툴을 설치합니다.
4.  **전략 실행:** Cline에게 초안을 시키고(`Draft`), 터미널의 `llm`으로 검증(`Refine`)하십시오. 이것이 2026년 전략의 미리보기입니다.
