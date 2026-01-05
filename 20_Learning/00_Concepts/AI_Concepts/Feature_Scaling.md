# Feature Scaling (특성 스케일링)

**Category**: #concept/ml/preprocessing
**Source**: [[24_ML_Preprocessing_Encoding_Scaling]]

## 정의
특성들의 **스케일(범위)을 조정**하여 모델 학습을 개선하는 기법입니다.

## 주요 방법

### StandardScaler (표준화)
평균 0, 표준편차 1로 변환

$$z = \frac{x - \mu}{\sigma}$$

- 정규분포 가정
- 이상치에 민감

### MinMaxScaler (정규화)
0과 1 사이로 변환

$$x' = \frac{x - x_{min}}{x_{max} - x_{min}}$$

- 범위가 명확할 때 유용
- 이상치에 매우 민감

## 필요한 경우
- 거리 기반 알고리즘 (KNN, SVM, K-Means)
- Gradient Descent 사용 시

## 주의사항
- Train 데이터로 fit, Test 데이터는 transform만!

## 관련 개념
- [[Data Leakage]]
- [[Feature Engineering]]
