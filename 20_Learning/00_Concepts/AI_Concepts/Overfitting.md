# Overfitting (과적합)

**Category**: #concept/ml/fundamentals
**Source**: [[11_ML_Course_Basics]]

---

## 정의 (Definition)
모델이 **훈련 데이터에는 지나치게 잘 맞지만**, 새로운 데이터(테스트 데이터)에는 성능이 떨어지는 현상입니다.

## 비유 (Analogy)
시험 문제집의 답을 달달 외워서 그 문제집은 100점을 맞지만, 숫자만 바뀐 새로운 문제는 전혀 풀지 못하는 학생과 같습니다.

## 원인 (Causes)
1. **모델이 너무 복잡함**: 파라미터가 지나치게 많아 데이터의 노이즈까지 학습
2. **데이터가 부족함**: 학습 샘플 수가 적어 일반화 능력 부족
3. **학습 시간이 너무 김**: 반복 학습으로 훈련 데이터에 과도하게 최적화

## 해결 방법 (Solutions)
- **Regularization** (규제): L1/L2 규제로 모델 복잡도 제한
- **Cross-Validation** (교차 검증): 여러 데이터셋으로 검증
- **Early Stopping** (조기 종료): 검증 성능이 떨어지기 시작하면 학습 중단
- **Data Augmentation** (데이터 증강): 학습 데이터 양 늘리기
- **Dropout**: 신경망에서 일부 뉴런을 무작위로 비활성화

## 관련 개념 (Related Concepts)
- [[Regularization]]
- [[Cross-Validation]]
- [[Bias-Variance Tradeoff]]
