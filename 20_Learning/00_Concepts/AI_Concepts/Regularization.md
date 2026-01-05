# Regularization (규제)

**Category**: #concept/ml/optimization
**Source**: [[41_ML_Classification_and_Regression]]

## 정의
모델의 복잡도를 제한하여 **과적합을 방지**하는 기법입니다.

## 주요 방법

### L1 Regularization (Lasso)
- 가중치의 절대값 합을 페널티로 추가
- 불필요한 특성의 가중치를 **0으로 만듦** (자동 특성 선택)

### L2 Regularization (Ridge)
- 가중치의 제곱 합을 페널티로 추가
- 모든 가중치를 **0에 가깝게** 만들지만 완전히 0은 아님

### ElasticNet
- L1과 L2를 결합한 방식

## 관련 개념
- [[Overfitting]]
- [[Gradient Descent]]
