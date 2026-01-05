# Transfer Learning (전이 학습)

**Category**: #concept/dl/technique
**Source**: [[21_DL_Course_CNN]]

## 정의
**사전 학습된 모델**을 가져와 새로운 작업에 맞게 튜닝하는 기법입니다.

## 핵심 아이디어
"바닥부터 학습하지 말고, 이미 학습된 지식을 재사용하자"

## 프로세스
1. **Pre-trained Model**: ImageNet 등으로 학습된 모델 (ResNet, VGG 등)
2. **Feature Extraction**: 하위 층은 고정, 상위 층만 재학습
3. **Fine-tuning**: 전체 또는 일부 층을 새 데이터로 미세 조정

## 장점
- 적은 데이터로도 높은 성능
- 학습 시간 대폭 단축
- 일반화 성능 향상

## 관련 개념
- [[CNN]]
- [[Transformer]]
