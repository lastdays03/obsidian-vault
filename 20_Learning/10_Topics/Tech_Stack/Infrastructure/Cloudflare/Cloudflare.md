---
created: 2026-01-26
tags:
  - knowledge/topic
  - tech_stack/infrastructure
  - tech_stack/cdn
  - edge_computing
  - serverless
Source: User Prompt
---

# Cloudflare

## Definition
**Cloudflare**는 2009년 설립된 **글로벌 에지 네트워크 및 보안 플랫폼**으로, 인터넷 인프라의 핵심을 담당하고 있습니다. 2026년 기준 전 세계 300개 이상의 도시에 데이터센터를 보유하고 있으며, 단순한 CDN을 넘어 **Workers(서버리스)**, **Pages(호스팅)**, **Workers AI(엣지 AI)** 등 풀스택 개발 플랫폼으로 진화했습니다.

## Key Features (2026 Trends)
- **Cloudflare Workers**: V8 Isolates 기반의 초경량 엣지 서버로서, Cold Start가 거의 0ms에 가까우며 글로벌 분산 처리가 기본입니다.
- **Workers AI**: 엣지에서 Llama 3.1, FLUX.2 등 최신 AI 모델을 추론할 수 있는 플랫폼으로, 지연 시간과 비용을 획기적으로 낮췄습니다.
- **R2 Storage**: AWS S3와 호환되면서도 **Egress(데이터 전송) 비용이 0원**인 객체 스토리지입니다.
- **Cloudflare Pages**: Git과 연동되어 정적 사이트 및 풀스택 앱(Next.js, Astro 등)을 자동으로 빌드하고 배포합니다.
- **Security**: 세계 최대 규모의 DDoS 방어 및 WAF 기능을 제공하여 애플리케이션을 안전하게 보호합니다.

## Pricing (Free vs Paid)
- **Free**: 개인 및 소규모 프로젝트에 매우 관대 (Workers 10만 건/일, Pages 무제한).
- **Workers Paid ($5/mo)**: 저렴한 월정액으로 시작하며, 스케일링 시 비용 효율이 매우 높음 (AWS Lambda 대비).
- **R2**: 스토리지 비용만 청구되며, 데이터 전송료가 무료인 것이 최대 강점.

## Usage Example (Workers)
```js
// index.ts (Cloudflare Worker)
export default {
  async fetch(request, env, ctx) {
    return new Response("Hello from Edge!", {
      headers: { "content-type": "text/plain" },
    });
  },
};
```

## Comparison (vs Vercel)
| Feature     | Cloudflare            | Vercel           |
| ----------- | --------------------- | ---------------- |
| **Speed**   | Best (Global Edge)    | Great            |
| **Cost**    | Very Low (No Egress)  | Premium          |
| **Next.js** | Good (Worker-based)   | Native           |
| **AI**      | Native Edge Inference | AI SDK / Gateway |

## Links
- [[Tech_Stack_MOC]]
- [[Vercel]] (Competitor)
