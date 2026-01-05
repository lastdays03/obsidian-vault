# RNN (Recurrent Neural Network)

**Category**: #concept/dl/architecture
**Source**: [[20_DL_Fundamentals_CNN_RNN]]

## 정의
**시퀀스 데이터** (텍스트, 시계열, 음성)를 처리하는 신경망으로, 과거 정보를 기억합니다.

## 핵심 아이디어
- **순환 구조**: 은닉 상태(Hidden State)가 다음 시점의 입력으로 재사용
- **메모리**: 이전 정보를 현재 예측에 활용

## 한계
**Vanishing Gradient**: 시퀀스가 길어지면 과거 정보가 희석됨

## 해결책
- [[LSTM]]: 장기 의존성 문제 해결
- GRU: LSTM의 경량화 버전

## 관련 개념
- [[LSTM]]
- [[Transformer]]
