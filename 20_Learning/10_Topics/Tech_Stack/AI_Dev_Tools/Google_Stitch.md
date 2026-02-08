# 개념 (Concept): Google Stitch

**태그**: #knowledge/concept #topic/Tech_Stack/AI_Dev_Tools
**출처**: [[Google_Stitch]]

---

## 📖 정의 (Definition)
**Google Stitch**는 텍스트 프롬프트나 스케치를 입력하면 **UI 디자인과 실제 작동하는 프론트엔드 코드(React, Tailwind CSS 등)를 동시에 생성해주는 Google의 AI 도구**입니다.
*2025년 5월 Google I/O에서 발표되었으며, 개발자와 디자이너 간의 협업 장벽을 허무는 것을 목표로 합니다.*

---

## 💡 예시 (Example)
*(Text-to-UI 및 Sketch-to-UI)*

1.  **Text Input**: "커피 선물 거래를 위한 모바일 대시보드 만들어줘"
    -> **Result**: 디자인 시안 + React/Tailwind 코드 생성.
2.  **Sketch Input**: 화이트보드에 그린 와이어프레임 사진 업로드
    -> **Result**: 실제 레이아웃이 적용된 웹페이지 코드로 변환.

```bash
# Workflow Example
1. Idea sketching on paper -> Photo upload to Stitch
2. Stitch generates UI & React Code
3. Export code to Project (e.g., 'ACE' frontend)
4. Refine in IDE
```

---

## ⚖️ 비교 (Comparison)
| Feature | Google Stitch | v0.dev (Vercel) | Galileo AI | Figma (Original) |
| :--- | :--- | :--- | :--- | :--- |
| **Core Function** | Text/Sketch to **Code & Design** | Text to Code (React/Shadcn) | Text to UI Design (Figma) | Vector Design Tool |
| **Input** | Text, **Images (Sketch)** | Text, Images | Text | Manual Drawing |
| **Output** | React, HTML, Tailwind | React, HTML, Vue | Figma Editable File | Design File |
| **Strengths** | **Multimodal (Sketch)**, Google Ecosystem | Vercel Integration, Shadcn UI | High quality Design assets | Industry Standard for Design |

---

## 🔑 Key Insights
- **Gemini 2.5 Pro & Flash 기반**: Google의 최신 멀티모달 모델을 사용하여 이미지 인식(스케치)과 코드 생성 능력이 뛰어납니다.
- **솔로프리너를 위한 가속기**: 백엔드(FastAPI 등)는 직접 개발하되, 프론트엔드 UI/UX는 Stitch로 빠르게 초안을 잡아 개발 속도를 획기적으로 단축할 수 있습니다.
- **Figma 연동**: 단순 코드만 생성하는 것이 아니라, 디자인을 Figma로 내보내 디자이너가 추가 작업을 할 수 있도록 지원합니다.

> [!NOTE] 동명이인 서비스: Google Cloud Video Stitcher API
> 동영상 스트리밍 서버 사이드 광고 삽입(SSAI) API로, UI 도구인 Google Stitch와는 전혀 다른 서비스입니다.

## 📚 References
- [Google I/O 2025 Keynote](https://io.google/2025)
- [Google Labs](https://labs.google)
