# Bias-Variance Tradeoff (편향-분산 트레이드오프)

**Category**: #concept/ml/fundamentals
**Source**: [[11_ML_Course_Basics]]

## 정의
모델의 **편향(Bias)**과 **분산(Variance)** 사이의 균형을 맞추는 문제입니다.

## 구성 요소

### Bias (편향)
- 모델의 예측값과 실제값 사이의 차이
- **High Bias** = 과소적합 (Underfitting)
- 모델이 너무 단순함

### Variance (분산)
- 훈련 데이터가 바뀔 때 예측값이 얼마나 변하는지
- **High Variance** = 과적합 (Overfitting)
- 모델이 너무 복잡함

## Tradeoff
- Bias를 줄이면 Variance가 증가
- Variance를 줄이면 Bias가 증가
- **목표**: 둘 다 낮은 최적점 찾기

## 관련 개념
- [[Overfitting]]
- [[Regularization]]
- [[Cross-Validation]]
