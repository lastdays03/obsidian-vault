# LSTM (Long Short-Term Memory)

**Category**: #concept/dl/architecture
**Source**: [[20_DL_Fundamentals_CNN_RNN]]

## 정의
RNN의 **장기 의존성 문제**를 해결한 발전된 구조입니다.

## 핵심 구성 요소

### Cell State
정보를 장기적으로 간직하는 "고속도로"

### Gates (게이트)
- **Forget Gate**: 불필요한 과거 정보 삭제
- **Input Gate**: 새로운 중요 정보 저장
- **Output Gate**: 다음 상태로 보낼 정보 결정

## 장점
- 긴 시퀀스에서도 과거 정보 유지
- 텍스트 생성, 번역, 음성 인식 등에 활용

## 관련 개념
- [[RNN]]
- [[Transformer]]
