# Data Leakage (데이터 누수)

**Category**: #concept/ml/pitfalls
**Source**: [[43_ML_Model_Evaluation_Tuning]]

## 정의
**테스트 데이터의 정보가 훈련 과정에 유입**되어 모델 성능이 부풀려지는 현상입니다.

## 발생 원인

### 1. 전처리 순서 오류
```python
# ❌ 잘못된 예
scaler.fit(X)  # 전체 데이터로 fit
X_train, X_test = train_test_split(X)

# ✅ 올바른 예
X_train, X_test = train_test_split(X)
scaler.fit(X_train)  # 훈련 데이터만으로 fit
X_train_scaled = scaler.transform(X_train)
X_test_scaled = scaler.transform(X_test)
```

### 2. 미래 정보 사용
시계열 데이터에서 미래 값을 특성으로 사용

### 3. Target Leakage
타겟과 직접 관련된 특성 포함 (예: 대출 승인 여부 예측 시 '대출 금액' 사용)

## 결과
- 훈련/검증 성능은 매우 높음
- 실제 배포 시 성능 급락

## 관련 개념
- [[Train-Test Split]]
- [[Cross-Validation]]
- [[Feature Engineering]]
