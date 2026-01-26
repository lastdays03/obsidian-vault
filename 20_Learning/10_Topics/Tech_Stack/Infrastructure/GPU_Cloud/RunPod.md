---
created: 2026-01-26
tags:
  - knowledge/topic
  - infra/gpu_cloud
  - ai_and_data/tools
  - cloud/gpu
Source: User Prompt
---

# RunPod

## Definition
**RunPod**는 AI/ML 워크로드를 위한 클라우드 GPU 플랫폼입니다. 저렴한 비용으로 최신 GPU(H100, RTX 4090 등)를 대여하여 Stable Diffusion, LLM fine-tuning, 이미지 생성 등을 수행할 수 있습니다. 2026년 기준 AI 실무자와 개발자들에게 가장 인기 있는 GPU 클라우드 중 하나입니다.

## Key Features (2026)
- **High Performance GPUs**: H100, H200, RTX 4090 등 최신 라인업 보유.
- **Pay-as-you-go**: 분 단위/밀리초 단위 과금 및 저렴한 Spot 인스턴스 제공.
- **Template System**: ComfyUI, Ollama, vLLM 등을 클릭 한 번으로 배포 가능한 템플릿 지원.
- **Developer DX**: VS Code Remote-SSH, JupyterLab, 웹 터미널 지원.
- **Serverless GPU**: GPU 리소스를 API 엔드포인트로 즉시 배포 가능.

## Pricing Examples (2026.01)
| GPU Model     | VRAM  | Usage                      | Note        |
| ------------- | ----- | -------------------------- | ----------- |
| **RTX 4090**  | 24GB  | Image Gen / Small LLM      | 가성비 최고 |
| **A100 80GB** | 80GB  | LLM Fine-tuning            | 대형 모델용 |
| **H100/H2)0** | 80GB+ | Heavy Training / Inference | 최고 성능   |

## Selection Guide
- **개인/취미**: RTX 4090 Spot 추천.
- **LLM 학습**: A100 80GB 이상 추천.
- **API 배포**: RunPod Serverless 추천.

## Links
- [[Tech_Stack_MOC]]
- [[AI_and_Data_MOC]]
- [[SaaS]] (Cloud Business Model)
