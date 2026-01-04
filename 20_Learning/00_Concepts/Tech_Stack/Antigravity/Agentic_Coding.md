---
tags: [knowledge/concept, methodology/antigravity]
Up: [[Antigravity_MOC]]
---

# 개념 (Concept): Agentic Coding

**출처**: [[03_Claude_Skills_Methodology]]

---

## 📖 정의 (Definition)
**Agentic Coding**은 개발자가 직접 코드를 한 줄씩 작성하는 대신, 고수준의 **설계(Design)**와 **의도(Intent)**를 프롬프트로 제공하고, AI 에이전트가 구체적인 **구현(Implementation)**을 담당하는 협업 패러다임입니다.

---

## 💡 예시 (Example)
전통적인 코딩이 "Login 함수를 작성해"라고 명령하는 것이라면, Agentic Coding은 다음과 같습니다.
> "사용자 인증 시스템을 설계해줘. 보안을 위해 JWT를 사용하고, 비밀번호는 bcrypt로 해싱해야 해. 그리고 `User` 모델과 `AuthService` 클래스로 구조를 분리해서 구현해줘."

이 요청을 받은 에이전트는 파일 생성, 패키지 설치, 코드 작성, 테스트까지 주도적으로 수행합니다.

## 💡 Key Insights
- **Context is King**: 에이전트가 뛰어난 코더가 되려면 프로젝트의 맥락(Context)을 얼마나 잘 이해하고 있느냐가 관건입니다. `@Codebase`와 같은 RAG 기술이 핵심입니다.
- **검증의 중요성**: 코드를 직접 짜지 않으므로, 인간은 '작성자'에서 '검토자(Reviewer)'로 역할이 바뀝니다. 코드 리뷰 능력이 더욱 중요해집니다.
