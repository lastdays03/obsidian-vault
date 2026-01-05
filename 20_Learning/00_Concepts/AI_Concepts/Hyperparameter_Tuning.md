# Hyperparameter Tuning (하이퍼파라미터 튜닝)

**Category**: #concept/ml/optimization
**Source**: [[43_ML_Model_Evaluation_Tuning]]

## 정의
모델의 성능을 최적화하기 위해 **하이퍼파라미터**를 조정하는 과정입니다.

## 하이퍼파라미터 vs 파라미터
- **파라미터**: 학습으로 자동 결정 (가중치, 편향)
- **하이퍼파라미터**: 사람이 설정 (학습률, 트리 깊이 등)

## 주요 기법

### Grid Search
- 모든 조합을 시도
- 정확하지만 느림

### Random Search
- 무작위 조합 시도
- 빠르고 효율적

### Bayesian Optimization
- 이전 결과를 바탕으로 다음 시도 결정
- 가장 효율적

```python
from sklearn.model_selection import GridSearchCV

param_grid = {'n_estimators': [100, 200], 'max_depth': [3, 5, 7]}
grid_search = GridSearchCV(model, param_grid, cv=5)
grid_search.fit(X_train, y_train)
```

## 관련 개념
- [[Cross-Validation]]
- [[Overfitting]]
