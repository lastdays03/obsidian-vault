# MSE and RMSE

**Category**: #concept/ml/metrics
**Source**: [[41_ML_Classification_and_Regression]]

## MSE (Mean Squared Error)
예측값과 실제값 차이의 제곱의 평균입니다.

$$MSE = \frac{1}{n}\sum_{i=1}^{n}(y_i - \hat{y}_i)^2$$

### 특징
- 오차를 제곱하므로 **이상치(Outlier)에 민감**
- 큰 오차에 큰 페널티 부여

## RMSE (Root Mean Squared Error)
MSE에 루트를 씌운 값입니다.

$$RMSE = \sqrt{MSE}$$

### 장점
- 실제 값과 **단위가 같아** 해석이 용이
- 예: 집값 예측 시 RMSE=5000만원 → 평균 5000만원 오차

## 관련 개념
- [[R2 Score]]
