---
created: 2026-01-20
tags:
  - knowledge/topic
  - AI/LLM
  - API
  - Resource
source: User Input
---

# Free LLM API Limits 2026

## Introduction
2026년 1월 20일 기준 주요 무료 LLM API의 모델별 토큰 제한량 및 사용량 가이드입니다. 공식 문서와 사용자 경험을 바탕으로 정리되었습니다.

## Token Limits Summary

| 제공자 / 모델명                       | 무료 티어 한도 (상세)                              | 실제 체감 사용량 (2026-01)                         | API 키 발급                                          |
| :------------------------------ | :----------------------------------------- | :------------------------------------------ | :------------------------------------------------ |
| **Google Gemini API**           |                                            |                                             | [Get Key](https://aistudio.google.com/app/apikey) |
| - Gemini 2.5 Flash / Flash-Lite | TPM: 250,000<br>RPM: 5~15<br>RPD: 20~1,000 | 하루 20~100회 (짧은 대화)<br>긴 컨텍스트(1M) 테스트 10~30회 |                                                   |
| - Gemini 3 Flash Preview        | TPM: 250,000<br>RPM: 5~15<br>RPD: 20~50    | 하루 20~50회<br>테스트용 적합, 무거운 배치 어려움            |                                                   |
| **Groq**                        |                                            |                                             | [Get Key](https://console.groq.com/keys)          |
| - Llama 3.1 8B                  | TPM: 6,000<br>RPM: 30~50<br>RPD: 14,400    | 하루 5,000~10,000회<br>속도 초고속                  |                                                   |
| - Llama 3.3 70B                 | TPM: 12,000<br>RPM: 30~50<br>RPD: 1,000    | 하루 500~2,000회<br>고성능 모델                     |                                                   |
| - Gemma 2 9B Instruct           | TPM: 6k~14k<br>RPD: 1k~7k                  | 하루 1,000~5,000회                             |                                                   |
| **Together AI**                 |                                            |                                             | [Get Key](https://www.together.ai)                |
| - Llama-3.3-70B-Free            | TPM/RPM: 600~3,000                         | 하루 수백~수천 요청                                 |                                                   |
| - DeepSeek-R1-Free              | 낮은 Rate Limit                              | 하루 500~2,000회                               |                                                   |
| **OpenRouter**                  |                                            |                                             | [Get Key](https://openrouter.ai/keys)             |
| - Free Models (18+)             | 50 req/day (기본)<br>$10 충전시 1,000 req/day   | 기본 50회/일<br>충전 시 실질 무제한                     |                                                   |
| **Hugging Face**                |                                            |                                             | [Get Key](https://huggingface.co/settings/tokens) |
| - Inference API (300+)          | 월 1,000~10,000 req                         | 하루 30~300회<br>소규모 테스트용                      |                                                   |
| **DeepSeek API**                |                                            |                                             | [Get Key](https://platform.deepseek.com/api_keys) |
| - V3 / R1                       | 신규 5M 토큰 (30일 유효)                          | 초기 30일 테스트 최적 (Fair-use)                    |                                                   |

## Key Insights & Tips
*   **Google Gemini**: 한국어 및 멀티모달 성능이 가장 우수하나, 2025년 12월 이후 Quota가 대폭 축소되었습니다(RPD 20~50 수준).
*   **Groq**: 압도적인 속도와 관대한 무료 쿼터(Llama 3.1 8B RPD 14,400)를 제공합니다.
*   **Together AI / OpenRouter**: 무료 전용 모델 활용 시 실질적으로 무제한에 가까운 사용이 가능합니다.
*   **Hugging Face**: 요청 수 중심의 제한으로 짧은 테스트에 적합합니다.
*   **DeepSeek**: 가입 시 제공되는 5M 토큰 보너스가 강력하여 초기 테스트에 유리합니다.

## Best Recommendations (Korean Context)
한국어 작업을 위한 추천 조합:
1.  **Google Gemini 2.5 Flash**: 언어 처리 능력 최우수.
2.  **Groq (Llama 3.3 70B)**: 높은 성능과 준수한 속도 밸런스.

---
**Note**: Quota는 변동될 수 있으므로 각 제공자의 대시보드에서 최신 정보를 확인하세요.
