# Ensemble Learning (앙상블 학습)

**Category**: #concept/ml/algorithms
**Source**: [[51_ML_Ensemble_Learning]]

## 정의
여러 개의 약한 모델(Weak Learner)을 결합하여 강한 모델(Strong Learner)을 만드는 기법입니다.

## 주요 방법

### Voting
서로 다른 알고리즘의 예측을 투표로 결정

### Bagging (Bootstrap Aggregating)
- 같은 알고리즘, 다른 데이터 샘플
- 대표: **Random Forest**

### Boosting
- 이전 모델의 오류를 다음 모델이 보완
- 대표: **XGBoost, LightGBM**

### Stacking
여러 모델의 예측을 입력으로 사용하는 메타 모델

## 관련 개념
- [[Overfitting]]
- [[Bias-Variance Tradeoff]]
