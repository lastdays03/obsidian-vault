---
tags: [knowledge/topic, tool/claude]
Up: [[Claude_Code_MOC]]
---

# 04. Future Proofing

LLM의 가장 큰 약점은 **학습 데이터의 시의성(Cut-off date)**입니다. Future-Proofing 전략은 최신 라이브러리 문서를 실시간으로 학습하고, 버전을 검증하여 이러한 한계를 기술적으로 극복합니다.

## 📖 [[Docs-to-Skill]] 파이프라인

Browser MCP를 통해 최신 공식 문서를 크롤링하고, 이를 Claude가 이해하기 쉬운 `SKILL.md`로 변환하여 프로젝트 컨텍스트에 주입하는 기술입니다.

### Docs-to-Skill Prompt Example
```bash
claude -p "
Create a new skill named 'bun-expert' by researching official docs.

[Instructions]
1. Use 'browser_tools' to visit 'https://bun.sh/docs'.
2. Crawl 'Quick Start', 'API Reference'.
3. Synthesize information into 'SKILL.md'.
   - Installation
   - Server implementation
   - File I/O
"
```

## 🛡️ Version-Agnostic Coding

"내가 아는 문법이 틀릴 수 있다"는 가정 하에, 코드를 작성하기 전 반드시 버전을 확인하도록 강제하는 전략입니다.

### Version Check Skill (`version-check`)
```yaml
---
name: version-check
description: Mandates library version verification before implementation.
allowed-tools: ["read_file", "browser_tool"]
---

# Version Verification Protocol

## Rule
Before implementing code involving external libraries (Next.js, AWS SDK etc.):

1. **Identify Version**: Read `package.json`.
2. **Gap Analysis**: Compare with internal knowledge.
3. **Syntax Verification**:
   - Use `browser_tool` to search "migration guide {version}" or "breaking changes".
   - Confirm API syntax matches the installed version.

## Enforcement
Do NOT generate code until syntax is confirmed.
```

## 💡 Key Insights
- **Knowledge Freshness**: 지식에도 유통기한이 있다. `SKILL.md`는 LLM의 지식을 최신 상태로 유지하는 "패치(Patch)" 역할을 한다.
- **Trust but Verify**: 모델의 기억을 믿지 말고, 현재 설치된 `package.json`과 공식 문서를 믿어라.
- **Zero-Hallucination**: 버전을 먼저 확인하고 문법을 검색하는 단순한 절차가 환각에 의한 버그를 90% 이상 줄여준다.
