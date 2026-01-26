---
created: 2026-01-26
tags:
  - knowledge/topic
  - tech_stack/ai_tools
  - productivity/learning
  - google/notebooklm
Up: [[AI_Dev_Tools_MOC]]
Source: User Prompt
---

# NotebookLM

## Definition
**NotebookLM**은 Google이 제공하는 AI 기반 연구 및 학습 보조 도구입니다. 사용자가 직접 업로드한 소스 자료(PDF, 웹 URL, 유튜브 영상 등)만을 기반으로 답변을 생성하므로 환각(Hallucination) 현상이 매우 적으며, 복잡한 정보를 요약, 구성, 깊이 있게 학습하는 데 특화되어 있습니다.

## Key Features (2026)
- **Source-Centric AI**: 최대 50개의 소스(50만 단어)를 업로드하고, 오직 해당 자료만을 근거로 삼는 "환각 없는 비서".
- **Audio Overview**: 자료의 핵심 내용을 두 명의 AI 호스트가 팟캐스트 형식으로 대화하며 요약해주는 혁신적인 음성 서비스 (한국어 지원).
- **Study Guide & Briefing**: Study Guide, FAQ, 연대표(Timeline) 등을 자동으로 생성하여 학습 및 보고서 작성을 보조.
- **Multimodal Support**: 도서 PDF, 유튜브 강의 자막, 오디오 파일 등 다양한 형태의 입력을 통합 분석.
- **Collaboration**: 노트북 단위로 링크를 공유하여 팀원들과 함께 자료를 공유하고 AI와 대화 가능.
- **Free to Use**: Gemini 2.5 Pro 모델을 기반으로 하며 완전 무료 제공.

## Comparison (2026)
| Feature           | NotebookLM              | ChatGPT / Claude        | Perplexity         |
| ----------------- | ----------------------- | ----------------------- | ------------------ |
| **Base Info**     | User Provided Only      | Mixed (Internal + Web)  | Web Search Centric |
| **Audio Summary** | **Excellent (Podcast)** | Limited                 | No                 |
| **Hallucination** | **Very Low**            | Varies                  | Usually Low        |
| **Primary Use**   | Research & Learning     | Genral Purpose / Coding | Fast Discovery     |

## Quick Start
1. **Access**: [notebooklm.google.com](https://notebooklm.google.com) 접속.
2. **Sources**: 연구 논문, 유튜브 URL, 회의록 등을 업로드.
3. **Actions**: 
    - "Study Guide 생성" 클릭하여 지식 구조화.
    - "Audio Overview" 생성하여 이동 중 청취.
    - 채팅창에 구체적인 내용 대조 질문 수행.

## Links
- [[AI_Dev_Tools_MOC]]
- [[Tech_Stack_MOC]]
- [[NotebookLM_Agency_Workflow_Guide]] (Practical Guide)
- [[RAG_LLM_Setup_Guide_M1_Max]] (Comparison point)
