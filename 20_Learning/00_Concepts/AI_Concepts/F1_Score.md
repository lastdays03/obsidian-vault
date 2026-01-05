# F1 Score

**Category**: #concept/ml/metrics
**Source**: [[42_ML_Course_Evaluation_Metrics]]

## 정의
**Precision**과 **Recall**의 **조화 평균**입니다.

$$F1 = 2 \times \frac{\text{Precision} \times \text{Recall}}{\text{Precision} + \text{Recall}}$$

## 특징
- 두 지표의 균형을 평가
- 한쪽이 극단적으로 낮으면 F1도 낮아짐
- 불균형 데이터에서 Accuracy보다 신뢰할 수 있음

## 사용 시기
- Precision과 Recall이 모두 중요한 경우
- 단일 지표로 모델 성능을 비교할 때

## 관련 개념
- [[Precision]]
- [[Recall]]
- [[ROC-AUC]]
