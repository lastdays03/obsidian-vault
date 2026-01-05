# Cross-Validation (교차 검증)

**Category**: #concept/ml/evaluation
**Source**: [[43_ML_Model_Evaluation_Tuning]]

---

## 정의 (Definition)
데이터를 여러 부분으로 나누어 **번갈아가며 훈련과 검증을 수행**하여 모델의 일반화 성능을 평가하는 기법입니다.

## 핵심 아이디어 (Key Idea)
단 한 번의 Train-Test Split은 운이 좋거나 나쁜 분할일 수 있습니다. 여러 번 검증하여 평균을 내면 더 신뢰할 수 있는 성능 추정이 가능합니다.

## 주요 방법 (Methods)

### K-Fold Cross-Validation
1. 데이터를 K개의 폴드(Fold)로 분할
2. 각 폴드를 한 번씩 검증 세트로 사용
3. 나머지 K-1개 폴드로 훈련
4. K번의 결과를 평균내어 최종 성능 산출

```python
from sklearn.model_selection import cross_val_score

scores = cross_val_score(model, X, y, cv=5)  # 5-Fold CV
print(f"평균 정확도: {scores.mean():.3f}")
```

### Stratified K-Fold
클래스 비율을 유지하면서 분할하는 방법. **불균형 데이터**에 필수적입니다.

## 장점 (Advantages)
- 모든 데이터가 훈련과 검증에 사용됨
- 과적합 여부를 더 정확히 판단 가능
- 데이터가 적을 때 특히 유용

## 단점 (Disadvantages)
- 계산 비용이 K배 증가
- 시계열 데이터에는 부적합 (시간 순서 무시)

## 관련 개념 (Related Concepts)
- [[Train-Test Split]]
- [[Overfitting]]
- [[Hyperparameter Tuning]]
