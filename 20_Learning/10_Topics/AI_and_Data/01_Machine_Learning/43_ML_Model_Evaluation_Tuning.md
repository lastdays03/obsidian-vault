---
Source: study_machine_learning
---

# 모델 평가와 하이퍼파라미터 튜닝

## 1. 개요 및 목표
1.  **과적합(Overfitting)**의 위험성을 직접 확인합니다.
2.  이를 방지하기 위한 **교차 검증(Cross Validation)** 기법들(K-Fold, Stratified K-Fold)을 실습합니다.
3.  최적의 성능을 내는 하이퍼파라미터를 찾기 위해 **GridSearchCV**를 사용합니다.

## 1. 과적합(Overfitting)의 함정
**Why**: 학습 데이터로 그대로 시험을 치면 100점이 나옵니다. 하지만 이건 '암기'이지 '학습'이 아닙니다. 새로운 데이터에서는 틀릴 수 있습니다.

```python
from sklearn.tree import DecisionTreeClassifier
from sklearn.metrics import accuracy_score

# 학습 데이터로만 학습하고 예측하면?
dt_clf = DecisionTreeClassifier(random_state=123)
dt_clf.fit(X, y)
pred = dt_clf.predict(X)

print(f"학습 데이터로 평가한 정확도: {accuracy_score(y, pred):.4f} (완벽해보이지만 위험함)")
```
> **Output**: 학습 데이터로 평가한 정확도: 1.0000 (완벽해보이지만 위험함)

## 2. 학습/테스트 데이터 분리
가장 기본적인 일반화 성능 검증 방법입니다.

```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=121)

dt_clf.fit(X_train, y_train)
pred = dt_clf.predict(X_test)
print(f"테스트 데이터로 평가한 정확도: {accuracy_score(y_test, pred):.4f}")
```
> **Output**: 테스트 데이터로 평가한 정확도: 0.9556

## 3. 교차 검증 (K-Fold Cross Validation)
**Why**: 데이터가 적을 때, 테스트 데이터가 우연히 쉬운 문제들만 뽑혔을 수도 있습니다. 데이터를 K개로 쪼개서 번갈아가며 다 테스트해보는 방법입니다. (데이터를 알뜰하게 사용)

```python
from sklearn.model_selection import KFold
import numpy as np

kfold = KFold(n_splits=5)
cv_accuracy = []

print("--- K-Fold 시작 ---")
for i, (train_idx, test_idx) in enumerate(kfold.split(X)):
    X_train_cv, X_test_cv = X[train_idx], X[test_idx]
    y_train_cv, y_test_cv = y[train_idx], y[test_idx]
    
    dt_clf.fit(X_train_cv, y_train_cv)
    pred_cv = dt_clf.predict(X_test_cv)
    acc = accuracy_score(y_test_cv, pred_cv)
    
    cv_accuracy.append(acc)
    print(f"#{i+1} 검증 정확도: {acc:.4f}")

print(f"\n평균 정확도: {np.mean(cv_accuracy):.4f}")
```
> **Output**: 평균 정확도: 0.9200

## 4. Stratified K-Fold
**Why**: 붓꽃 데이터처럼 레이블이 불균형하거나 분류 문제일 때는 K-Fold가 위험할 수 있습니다. (특정 품종이 학습셋에 아예 없을 수도 있음). Stratified K-Fold는 정답 비율을 유지하면서 나눕니다.

```python
from sklearn.model_selection import StratifiedKFold

skf = StratifiedKFold(n_splits=3)
cv_accuracy = []

print("--- Stratified K-Fold 시작 ---")
# split할 때 y(레이블)를 넣어줘야 비율을 맞출 수 있음
for i, (train_idx, test_idx) in enumerate(skf.split(X, y)):
    X_train_cv, X_test_cv = X[train_idx], X[test_idx]
    y_train_cv, y_test_cv = y[train_idx], y[test_idx]
    
    dt_clf.fit(X_train_cv, y_train_cv)
    acc = accuracy_score(y_test_cv, dt_clf.predict(X_test_cv))
    
    cv_accuracy.append(acc)
    print(f"#{i+1} 검증 정확도: {acc:.4f}")

print(f"\nStratified 평균 정확도: {np.mean(cv_accuracy):.4f}")
```
> **Output**: Stratified 평균 정확도: 0.9667

## 5. GridSearchCV (하이퍼파라미터 튜닝 + 교차검증)
**Why**: 모델의 성능을 높이려면 파라미터(트리 깊이 등)를 조절해야 합니다. 일일이 해보기 힘드니, 범위를 지정해주면 알아서 최적의 조합을 찾아줍니다.

```python
from sklearn.model_selection import GridSearchCV

# 테스트할 파라미터 조합
params = {
    'max_depth': [1, 2, 3],
    'min_samples_split': [2, 3]
}

# GridSearchCV 객체 생성 (refit=True: 최적 파라미터로 다시 학습시켜둠)
grid_dtree = GridSearchCV(dt_clf, param_grid=params, cv=3, refit=True)

# 튜닝 수행
grid_dtree.fit(X_train, y_train)

print(f"최적 파라미터: {grid_dtree.best_params_}")
print(f"최고 정확도: {grid_dtree.best_score_:.4f}")

# 최적 모델로 최종 테스트
best_model = grid_dtree.best_estimator_
final_pred = best_model.predict(X_test)
print(f"최종 테스트 정확도: {accuracy_score(y_test, final_pred):.4f}")
```
> **Output**:
> 최적 파라미터: {'max_depth': 3, 'min_samples_split': 2}
> 최고 정확도: 0.9429
> 최종 테스트 정확도: 0.9556

## 6. 인사이트 도출 (Insights)

### 🌟 Key Takeaways
*   **과적합(Overfitting)의 위험**: 학습 데이터로 평가했을 때의 100% 정확도는 '실력'이 아니라 '암기'일 수 있음을 확인했습니다.
*   **교차 검증(Cross Validation)**: 데이터를 여러 번 쪼개서 검증함으로써, 우연에 의한 성능 뻥튀기를 방지하고 모델의 **일반화 성능**을 신뢰할 수 있게 되었습니다.
*   **Stratified K-Fold**: 분류 문제, 특히 정답 비율이 불균형할 때는 반드시 정답 비율을 유지하며 쪼개는 **Stratified** 방식이 필요합니다.
*   **GridSearchCV**: 최적의 하이퍼파라미터를 찾는 과정을 자동화하여, 수동 노가다 없이 성능을 1%라도 더 끌어올릴 수 있었습니다.

### 🔬 Try More
*   **파라미터 범위 확장**: `max_depth`를 `[1, 2, 3]`이 아니라 `[1, 3, 5, 10, None]` 등으로 넓혀서 실험해보세요.
*   **scoring 변경**: `accuracy` 대신 `f1`이나 `roc_auc` 점수를 기준으로 최적 모델을 찾아보세요. (단, 이진 분류인 경우 주의 필요)
