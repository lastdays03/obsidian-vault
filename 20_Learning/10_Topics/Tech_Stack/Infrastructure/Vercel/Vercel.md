---
created: 2026-01-26
tags:
  - knowledge/topic
  - tech_stack/infrastructure
  - tech_stack/deployment
  - vercel
  - nextjs
Source: User Prompt
---

# Vercel

## Definition
**Vercel**은 **프론트엔드 중심의 클라우드 플랫폼**으로, Next.js의 개발사입니다. "Develop. Preview. Ship."이라는 슬로건 아래, 복잡한 서버 설정 없이 Git Push만으로 자동 빌드 및 배포가 가능한 **Zero-Config** 경험을 제공합니다. 2026년 기준 **AI 통합(v0, AI SDK)**과 **Edge Network**를 강화하여 AI 시대를 이끄는 핵심 인프라로 자리 잡았습니다.

## Key Features (2026 Trends)
- **Zero-Config Deployment**: GitHub/GitLab과 연동하여 커밋 즉시 프로덕션/프리뷰 배포가 완료됩니다.
- **Framework Native**: Next.js, SvelteKit, Astro 등 최신 프레임워크의 기능을(App Router, Server Actions) 별도 설정 없이 100% 지원합니다.
- **Edge Network**: 전 세계에 분산된 엣지 네트워크를 통해 정적 자산과 엣지 함수를 초저지연으로 제공합니다.
- **AI Integration**: **v0**(Generative UI)와 **AI SDK**를 통해 자연어로 UI를 생성하고 AI 에이전트를 쉽게 배포할 수 있습니다.
- **Observability**: 배포된 앱의 실시간 로그, 성능 지표(Speed Insights), 사용자 경험(Web Vitals)을 통합 모니터링합니다.

## Pricing (Hobby vs Pro)
- **Hobby (Free)**: 개인 프로젝트용. 상업적 이용 불가하지만 기능은 강력함.
- **Pro ($20/member/mo)**: 팀 협업, 무제한 프로젝트, 더 높은 리소스 제한. (트래픽 급증 시 추가 과금 주의)

## Usage Example (Deploy)
1. Next.js 프로젝트를 GitHub에 Push.
2. Vercel 대시보드에서 'New Project' -> 저장소 선택.
3. 'Deploy' 클릭 (자동으로 프레임워크 감지 및 빌드).

## Comparison
| Feature  | Vercel         | Netlify | AWS Amplify |
| -------- | -------------- | ------- | ----------- |
| **DX**   | Best (Next.js) | Great   | Good        |
| **Edge** | Fast & Global  | Fast    | AWS Backed  |
| **AI**   | Leading (v0)   | Growing | AWS Bedrock |

## Links
- [[Tech_Stack_MOC]]
- [[Nextjs]] (Native Support)
- [[Vercel_Agent_Skills]] (AI Feature)
