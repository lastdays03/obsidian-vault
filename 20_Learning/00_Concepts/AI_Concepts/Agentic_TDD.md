---
tags: [concept/ai, methodology/tdd]
Up: [[AI_Concepts_MOC]]
---

# Agentic TDD

> **Agentic TDD**는 AI 에이전트에게 **테스트 주도 개발(Test-Driven Development)** 방법론을 강제하여, 환각(Hallucination)을 방지하고 코드의 정합성을 보장하는 자동화된 코딩 프로토콜입니다.

## 📖 Definition
인간 개발자가 수행하던 **Red-Green-Refactor** 사이클을 AI 에이전트가 스스로 수행하도록 위임하는 것입니다.
핵심은 프롬프트나 스킬(Skill)을 통해 "테스트가 실패하기 전에는 구현 코드를 작성하지 말 것"이라는 제약 조건을 AI에게 주입하는 데 있습니다. 이는 AI가 그럴듯해 보이는 가짜 코드(Hallucination)를 작성하는 것을 원천적으로 차단합니다.

## 💻 Protocol
1.  **Phase 1 (RED)**: AI가 요구사항을 분석하여 `*.test.ts`를 먼저 작성하고 실행함 -> 실패 확인.
2.  **Phase 2 (GREEN)**: AI가 테스트를 통과하기 위한 최소한의 `*.ts` 코드를 작성함 -> 성공 확인.
3.  **Phase 3 (REFACTOR)**: 기능 변경 없이 코드 구조를 개선함.

## 🆚 Comparison
| 특징 | Traditional TDD | Agentic TDD |
| :--- | :--- | :--- |
| **주체** | 인간 개발자 | AI 에이전트 |
| **목적** | 설계 개선, 버그 방지 | 환각 방지, 자율성 검증 |
| **속도** | 느림 (인간의 사고 속도) | 매우 빠름 (병렬 실행 가능) |

## 🔗 Connected Concepts
- [[TDD]]
- [[Agentic Workflow]]
- [[Prompt Engineering]]

Source: [[05_Enterprise_Architecture]]
