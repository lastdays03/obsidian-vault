# CNN (Convolutional Neural Network)

**Category**: #concept/dl/architecture
**Source**: [[20_DL_Fundamentals_CNN_RNN]]

## 정의
**이미지 처리**에 특화된 신경망으로, 공간적 패턴을 학습합니다.

## 핵심 구성 요소

### Convolution Layer
- **Filter (Kernel)**: 특정 패턴(선, 곡선 등)을 감지
- **Feature Map**: 필터가 이미지를 훑으며 생성한 특징 지도

### Pooling Layer
- **Max Pooling**: 영역에서 최대값만 추출
- 목적: 연산량 감소, 이동 불변성 확보

### Fully Connected Layer
추출된 특징으로 최종 분류 수행

## 특징
- 낮은 층: 선/점 같은 단순 특징
- 높은 층: 눈/코/입 같은 복합 특징

## 관련 개념
- [[Transfer Learning]]
- [[RNN]]
