---
tags: [tech/tool, topic/API, topic/Code_Generation]
Source: User Input
---

# OpenAPI Generator

**태그**: #tech/tool #topic/API #topic/Code_Generation
**관련**: [[Code_First_API_Strategy]], [[FastAPI]]

---

## 📝 정의 (Definition)
**OpenAPI Generator**는 **Contract-First (설계 주도)** 개발 방식의 표준 도구입니다. OpenAPI Specification (OAS, 구 Swagger) 파일을 기반으로 다양한 언어의 **API 클라이언트(SDK)**, **서버 스텁(Stub)**, **API 문서**를 자동으로 생성해줍니다.

---

## 💡 개발 방식 비교: Contract-First vs Code-First
이 도구는 **Contract-First** 방식의 핵심 구현체입니다.

### Contract-First (설계 주도)
*API 명세(YAML/JSON)를 먼저 정의하고, 이를 통해 코드를 생성하는 방식*
- **장점**: 
    - **명확한 계약(Contract)**: 클라이언트/서버 개발자가 명세서만 보고 병렬 개발 가능.
    - **언어 중립적**: 특정 언어(Python, Java 등)에 종속되지 않음.
- **대표 도구**: [[OpenAPI_Generator]] (Standard), Swagger Codegen (Legacy)

*(반면, **Code-First**는 [[FastAPI]]와 같이 코드를 짜면 문서가 나오는 방식입니다. 자세한 비교는 전략 가이드를 참고하세요.)*

---

## 🔦 도구 비교: vs Swagger Codegen
| 구분            | **OpenAPI Generator**          | **Swagger Codegen** |
| :-------------- | :----------------------------- | :------------------ |
| **상태 (2026)** | **Standard (주류)**            | Legacy (비추천)     |
| **유지보수**    | 활발한 커뮤니티 주도           | 업데이트 정체       |
| **기능**        | 더 많은 언어 및 최신 스펙 지원 | 제한적              |

> **결론**: **OpenAPI Generator**가 Swagger Codegen의 상위 호환이므로, 무조건 이것을 사용해야 합니다.

---

## 🚀 사용법 (Usage)

### 1. 설치 (Installation)
Docker 또는 Homebrew를 통해 설치하는 것이 일반적입니다.

```bash
# Homebrew (macOS)
brew install openapi-generator

# npm
npm install @openapitools/openapi-generator-cli -g
```

### 2. 코드 생성 예시 (Generate Code)
`openapi.yaml` 파일로부터 TypeScript 클라이언트 코드를 생성하는 명령어입니다.

```bash
openapi-generator generate \
  -i openapi.yaml \
  -g typescript-axios \
  -o ./generated-client
```

---

## 📊 요약 (Summary)
- **역할**: Contract-First 방식의 API 명세 기반 코드 생성기
- **주요 용도**: 프론트엔드/모바일용 **API 클라이언트 SDK 자동 생성**
- **권장 전략**: 백엔드([[FastAPI]])에서 생성된 `openapi.json`을 가져와서, 이 도구로 클라이언트 코드를 생성하는 **Hybrid Workflow**가 가장 효율적입니다.
