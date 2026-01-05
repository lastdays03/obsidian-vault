# Train-Test Split (학습-테스트 분할)

**Category**: #concept/ml/evaluation
**Source**: [[11_ML_Course_Basics]]

## 정의
데이터를 **훈련 세트(Training Set)**와 **테스트 세트(Test Set)**로 나누는 기본적인 검증 방법입니다.

## 목적
- 훈련 세트: 모델 학습용
- 테스트 세트: 모델 성능 평가용 (미래 데이터 시뮬레이션)

## 일반적인 비율
- **80:20** 또는 **70:30** (훈련:테스트)
- 데이터가 적으면 90:10도 사용

```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)
```

## 주의사항
- **Data Leakage** 방지: 테스트 데이터는 절대 학습에 사용 금지
- **Stratify**: 클래스 비율 유지 (불균형 데이터)
- **Random State**: 재현성을 위해 고정

## 관련 개념
- [[Cross-Validation]]
- [[Data Leakage]]
- [[Overfitting]]
