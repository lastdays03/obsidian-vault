---
tags: [concept/ai, model/inference]
Up: [[AI_Concepts_MOC]]
---

# Thinking Mode

> **Thinking Mode**는 LLM(Large Language Model)이 답변을 생성하기 전에, 내부적으로 "사고 과정(Chain of Thought)"을 먼저 생성하고 검증할 수 있도록 추가적인 토큰 예산을 할당하는 추론 전략입니다.

## 📖 Definition
기존 LLM은 입력에 대해 즉각적인 확률적 답변을 생성했습니다. 반면 **Thinking Mode**는 모델이 "잠시 멈춰서(Pause)" 문제를 단계별로 분해하고, 가설을 세우고, 코너 케이스를 검토하는 등의 내부 독백(Internal Monologue)을 수행하게 합니다. 
Claude 3.7부터는 `/think` 명령어를 통해 이 사고 과정의 깊이(Token Budget)를 명시적으로 제어할 수 있습니다.

## 💻 Example
Claude Code CLI에서의 활용 예시입니다.

```bash
# 기본 모드 (단순 질문)
claude -p "Fix this typo in README.md"

# Thinking Mode (리팩토링)
claude /think "Refactor this function to be more readable"

# Deep Thinking (아키텍처 설계)
claude /think hard "Design a hexagonal architecture for this user service"
```

## 🆚 Comparison
| 모드 | Token Budget | 활용 사례 |
| :--- | :--- | :--- |
| **Direct Response** | ~1k | 단순 질의응답, 문법 수정 |
| **Thinking** | 2k ~ 8k | 논리적 오류 검출, 코드 최적화 |
| **Extended Thinking** | 8k ~ 32k+| 복잡한 수학 문제, 법률 검토, 시스템 설계 |

## 🔗 Connected Concepts
- [[Chain of Thought (CoT)]]
- [[LLM Inference]]
- [[Claude_Code_MOC]]

Source: [[01_CLI_Foundation]]
