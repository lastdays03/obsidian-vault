---
Source: study_machine_learning
---

# 딥러닝 기초: CNN과 RNN
# 태그: #DeepLearning #CNN #RNN #LSTM #PyTorch

이 문서는 `C2_M2` 주차의 노트북들(`Project_CNN`, `Exp_RNN_Sentiment`)에서 다루어진 **딥러닝(Deep Learning)**의 핵심 아키텍처인 **CNN**과 **RNN**을 요약 정리한 것입니다.

---

## 1. CNN (Convolutional Neural Networks, 합성곱 신경망)
이미지 처리에 특화된 신경망 구조입니다. 인간의 오디오/비주얼 코텍스 구조에서 영감을 받았습니다.

### 1-1. 핵심 개념
*   **Convolution (합성곱) Layer**: 필터(커널)가 이미지를 훑으며 특징(Feature Map)을 추출합니다. (예: 세로선, 가로선, 곡선 등 감지)
*   **Pooling Layer**: 특징 맵의 크기를 줄여(Downsampling) 연산량을 감소시키고, 위치 변화에 대한 불변성(Invariance)을 확보합니다. (주로 Max Pooling 사용)
*   **Fully Connected (FC) Layer**: 추출된 특징들을 1차원으로 펴서(Flatten) 최종 분류를 수행합니다.

### 1-2. PyTorch 구현 예시
```python
import torch.nn as nn

class SimpleCNN(nn.Module):
    def __init__(self):
        super(SimpleCNN, self).__init__()
        # 특징 추출기 (Feature Extractor)
        self.features = nn.Sequential(
            nn.Conv2d(3, 16, kernel_size=3, padding=1), # 입력 채널 3(RGB) -> 출력 16
            nn.ReLU(),
            nn.MaxPool2d(2, 2) # 이미지 크기 절반으로 축소
        )
        # 분류기 (Classifier)
        self.classifier = nn.Linear(16 * 16 * 16, 10) # 예시 크기

    def forward(self, x):
        x = self.features(x)
        x = x.view(x.size(0), -1) # Flatten
        x = self.classifier(x)
        return x
```

---

## 2. RNN (Recurrent Neural Networks, 순환 신경망)
순서가 있는 **시퀀스(Sequence) 데이터**(텍스트, 시계열, 음성 등) 처리에 특화된 구조입니다.

### 2-1. 핵심 개념
*   **순환 구조**: 은닉 상태(Hidden State)가 다음 시점(Time Step)의 입력으로 다시 들어갑니다. 즉, **과거의 정보를 기억**합니다.
*   **한계 (Vanishing Gradient)**: 시퀀스가 길어지면 과거의 정보가 점차 희석되어 사라지는 장기 의존성(Long-term dependency) 문제가 발생합니다.

### 2-2. LSTM (Long Short-Term Memory)
RNN의 망각 문제를 해결하기 위해 고안된 발전된 구조입니다.
*   **Cell State**: 정보를 장기적으로 간직하는 고속도로 역할을 합니다.
*   **Gates**: 정보의 흐름을 제어합니다.
    *   **Forget Gate**: 불필요한 과거 정보를 지웁니다.
    *   **Input Gate**: 새로운 중요한 정보를 저장합니다.
    *   **Output Gate**: 다음 상태로 보낼 정보를 결정합니다.

```python
import torch.nn as nn

# 임베딩 차원 10, 은닉층 크기 8인 LSTM
lstm = nn.LSTM(input_size=10, hidden_size=8, batch_first=True)

# 입력 데이터 (배치 크기, 시퀀스 길이, 특징 차원)
# output: 모든 시점의 은닉 상태들
# (hn, cn): 마지막 시점의 은닉 상태(hidden)와 셀 상태(cell)
output, (hn, cn) = lstm(input_data)
```

---

## 3. 인사이트 (Insights)
*   **데이터 타입에 따른 선택**:
    *   **이미지(공간 정보)** -> **CNN** (지역적 패턴 감지)
    *   **텍스트/시계열(시간 정보)** -> **RNN/LSTM** (순차적 맥락 파악)
*   **발전 방향**: 최근에는 텍스트 처리 분야에서 RNN 계열보다 **Transformer(Attention)** 모델이 압도적인 성능을 보이며 주류로 자리 잡았습니다.
