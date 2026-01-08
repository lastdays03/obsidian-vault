---
tags:
  - knowledge/topic
  - tech_stack/claude
  - ai_architecture
source: "User Created (2026-01-08)"
Up: [[Claude_Code_MOC]]
---

# 합성 지능의 오케스트레이션: Claude Code, MCP, 에이전트 워크플로우를 위한 포괄적 프레임워크
*(Synthetic Intelligence Orchestration: A Comprehensive Framework)*

---

## 1. 서론: 에이전트형 CLI 환경으로의 패러다임 전환

소프트웨어 엔지니어링의 지형이 단순한 자동 완성이나 채팅 인터페이스 중심의 "AI 보조 코딩(AI-assisted coding)" 시대를 넘어, **"에이전트형 개발([[Agentic_Coding|Agentic Development]])"**의 시대로 지각 변동을 겪고 있습니다. 이러한 진화의 최전선에 있는 [[Claude_Code_Handbook|Claude Code]]는 개발자와 대규모 언어 모델(LLM) 간의 관계를 근본적으로 재정의하는 도구입니다.

통합 개발 환경(IDE) 내 샌드박스에 갇힌 플러그인 형태로 작동하던 이전 도구들과 달리, Claude Code는 터미널에 네이티브로 상주하는 정교한 **REPL(Read-Eval-Print Loop)**로 설계되었습니다. 이것은 단순한 외형적 차이가 아니라, AI에게 운영 체제의 도구, 파일 시스템, 실행 환경에 대한 직접적인 권한을 부여하는 *"Give Claude a Computer"* 설계 원칙으로의 철학적 전환을 의미합니다.

이러한 변화의 함의는 매우 큽니다. 전통적인 워크플로우에서 인간 개발자는 컨텍스트를 채팅창에 수동으로 복사하고, AI의 출력을 해석하여 변경 사항을 적용하는 오케스트레이터(지휘자) 역할을 수행했습니다. 반면, Claude Code 패러다임에서 AI는 디렉터리 구조를 탐색하고, 문서를 읽고, 테스트를 실행하고, 코드를 커밋할 수 있는 자율적인 운영자(Operator), 즉 합성된 "시니어 개발자"의 역할을 맡습니다.

그러나 이러한 자율성에는 엄격한 거버넌스와 방향성이 필요합니다. 개발자의 역할은 "코드 작성자"에서 AI가 활동하는 환경을 조율하는 **"에이전트 아키텍트"**로 격상됩니다. 이 보고서는 이러한 환경을 **조율(Orchestration)**하는 방법에 대한 기술적 분석을 제공합니다.

---

## 2. 핵심 아키텍처: 에이전트 루프와 컨텍스트 관리

### 2.1 에이전트형 REPL 루프

Claude Code를 이해하려면 핵심 작동 루프를 분석해야 합니다. 상태가 없는(Stateless) 요청-응답 모델과 달리, Claude Code는 인간 개발자의 작업 기억(Working Memory)을 모방한 지속적인 세션 상태를 유지합니다. 이 루프는 **컨텍스트 수집(Gather Context) -> 행동 수행(Take Action) -> 검증(Verify Work) -> 반복(Repeat)**의 4단계를 거칩니다.

1. **컨텍스트 수집**: 프롬프트를 받으면 에이전트는 사용 가능한 도구, 현재 파일 시스템 상태, 로드된 구성 파일(`CLAUDE.md`, 설정 등)을 스캔합니다.
2. **추론 및 계획**: 모델은 "생각의 사슬(Chain of Thought)"(또는 "Extended Thinking" 모드)을 통해 필요한 단계를 결정합니다.
3. **행동 실행**: 에이전트는 명령을 내립니다. `grep` 검색, `sed`를 통한 파일 편집, 또는 MCP를 통한 외부 API 호출 등이 포함됩니다. 중요한 점은 이러한 작업이 사용자의 로컬 머신에서 실행된다는 것입니다.
4. **검증 및 피드백**: 에이전트는 명령의 stdout/stderr를 분석합니다. 오류가 발생하면 사람의 개입 없이 문제를 해결하려는 자가 수정(Self-correction) 하위 루프에 진입합니다.

```mermaid
sequenceDiagram
    participant U as User
    participant A as Claude Agent
    participant S as System (Terminal/MCP)

    U->>A: Prompt (Goal)
    loop Agentic Cycle
        A->>S: Gather Context (ls, read config)
        S-->>A: Context Data
        A->>A: Chain of Thought (Plan)
        A->>S: Action (grep, edit, commit)
        S-->>A: Result (stdout/stderr)
        alt Error
            A->>A: Self-Correction
        else Success
            A->>U: Verification Request / Result
        end
    end
```

> [!NOTE] Orchestration Strategy
> 환경을 올바르게 구성함으로써 우리는 1단계에서 특정 컨텍스트를 주입하고, 3단계에서 위험한 행동을 제한하며, 4단계에서 검증을 자동화할 수 있습니다.

### 2.2 구성(Configuration)과 기억의 계층 구조

에이전트 워크플로우의 주요 과제 중 하나는 긴 세션 동안 모델이 프로젝트 제약 조건을 잊어버리는 **"컨텍스트 드리프트(Context Drift)"** 현상입니다. Claude Code는 "프로젝트 메모리"를 확립하는 계층적 구성 시스템을 통해 이를 해결합니다.

| 구성 파일                 | 범위          | 주요 기능                                                                           | 오케스트레이션 전략                                                                                                               |
| :------------------------ | :------------ | :---------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------- |
| `~/.claude/settings.json` | 전역 (사용자) | 에이전트의 "기본 현실(Base Reality)" 정의: API 키, 전역 권한, 사용자별 도구 선호도. | 명확한 사용자 선호도(예: "항상 vim 모드 사용") 및 전역 보안 경계(예: "rm -rf 절대 금지") 설정에 사용.                             |
| `~/.claude/CLAUDE.md`     | 전역 (사용자) | "사용자 프로필". 모든 프로젝트에 적용되는 선호 사항 포함.                           | 고수준 코딩 습관 저장(예: "변수명은 간결하게", "항상 JSDoc 작성").                                                                |
| `.claude/settings.json`   | 프로젝트      | "프로젝트 거버넌스". Git에 커밋되어 공유되는 설정.                                  | 팀 전체의 도구 화이트리스트(`allowedTools`) 강제 및 특정 MCP 서버를 저장소에 바인딩.                                              |
| `CLAUDE.md` (Root)        | 프로젝트      | **"헌법(Constitution)"**. 기술 스택, 아키텍처, 불변의 규칙 정의.                    | 가장 중요한 파일. 시스템 프롬프트 주입 역할을 함. 간결하게 유지하고, 모든 문서를 저장하기보다 다른 문서를 가리키는 인덱스로 사용. |
| `CLAUDE.local.md`         | 로컬          | "연습장(Scratchpad)". Git에서 무시되는 로컬 컨텍스트.                               | 임시 메모, 로컬 테스트용 API 토큰, 현재 개발자만 기억해야 할 작업 목록 등에 사용.                                                 |
| 하위 디렉토리 `CLAUDE.md` | 중첩          | "문맥적 기억(Contextual Memory)".                                                   | 모노레포에서 `packages/frontend/`에 별도 `CLAUDE.md`를 배치하면 폴더 이동 시 자동으로 컨텍스트 전환.                              |

**CLAUDE.md 대 .clauderules 논쟁:**
최근 커뮤니티 연구에 따르면 지시 사항 처리 방식에 차이가 있음이 밝혀졌습니다. `CLAUDE.md`가 코드베이스 온보딩의 표준이지만, 모델은 이를 단순 컨텍스트로 취급하여 당면 과제와 무관하다고 판단하면 무시할 수 있습니다. 

> [!TIP] Progressive Disclosure
> 최적의 전략은 **"점진적 공개(Progressive Disclosure)"**입니다. `CLAUDE.md`에 프로젝트의 모든 내용을 담지 말고, 필요할 때 특정 `SKILL.md` 파일이나 문서 폴더를 읽도록 지시하는 인덱스 역할을 하게 해야 합니다.

---

## 3. 손과 발: 모델 컨텍스트 프로토콜 (MCP)

### 3.1 도구 사용을 위한 범용 인터페이스

[[02_MCP_Ecosystem|MCP(Model Context Protocol)]]는 Claude Code를 외부 세계와 연결하는 척추와 같습니다. 이는 N개의 AI 모델을 M개의 외부 도구에 연결하는 "N-to-M" 문제를 클라이언트-호스트-서버 토폴로지로 표준화하여 해결합니다.

| 기능          | Stdio 전송 (Stdio Transport)                         | SSE 전송 (Server-Sent Events)                  |
| :------------ | :--------------------------------------------------- | :--------------------------------------------- |
| **통신**      | 로컬 파이프를 통한 표준 입출력(Input/Output) 스트림. | 네트워크 소켓을 통한 HTTP/HTTPS.               |
| **지연 시간** | 매우 낮음 (In-process/로컬).                         | 중간 (네트워크 오버헤드).                      |
| **복잡성**    | 낮음 (Claude가 서버 수명 주기 관리).                 | 높음 (별도의 서버 프로세스 관리 필요).         |
| **보안**      | 높음 (서버가 자식 프로세스로 실행, 포트 개방 없음).  | 가변적 (인증/TLS 필요, 포트 노출).             |
| **사용 사례** | 로컬 개발 도구, CLI 래퍼, Docker 컨테이너.           | 원격 서버, 공유 팀 인스턴스, 장기 실행 서비스. |

대부분의 로컬 오케스트레이션 작업에는 단순성과 보안 프로필 때문에 Stdio가 선호됩니다. 개발자가 터미널을 닫으면 데이터베이스나 도구 연결이 깔끔하게 끊어지도록 보장합니다.

### 3.2 Docker화된 MCP 서버 오케스트레이션

호스트 머신에서 직접 MCP 서버를 실행하는 것은 "의존성 지옥(Dependency Hell)"을 초래할 수 있습니다. 견고한 오케스트레이션을 위한 모범 사례는 모든 MCP 서버를 Docker 컨테이너 내에 캡슐화하는 것입니다.

**설정 심층 분석: GitHub MCP**
```json
{
  "mcpServers": {
    "github": {
      "command": "docker",
      "args": ["run", "-i", "--rm", "-e", "GITHUB_PERSONAL_ACCESS_TOKEN", "mcp/github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${GITHUB_TOKEN}" // 변수 확장 구문
      }
    }
  }
}
```
* **아키텍처 노트**: `-i` 플래그는 필수입니다(Non-negotiable). 이 플래그가 없으면 컨테이너가 시작되자마자 종료되거나 Claude Code의 JSON-RPC 메시지를 수신하지 못합니다. `${GITHUB_TOKEN}` 사용은 저장소에 비밀 정보를 하드코딩하는 것을 방지하여 DevSecOps 모범 사례를 준수합니다.

### 3.3 데이터베이스 오케스트레이션: Postgres MCP

Claude를 데이터베이스에 연결하면 코더에서 DBA로 변모합니다. 이 통합을 통해 에이전트는 SQL 마이그레이션을 작성할 뿐만 아니라, 라이브 스키마에 대해 이를 검증하고 쿼리 성능을 분석할 수 있습니다.

```json
{
  "postgres": {
    "command": "docker",
    "args": ["run", "-i", "--rm", "-e", "POSTGRES_URL", "--network", "host", "mcp/postgres"],
    "env": {
      "POSTGRES_URL": "postgres://readonly_user:securepass@localhost:5432/dev_db"
    }
  }
}
```
> [!WARNING] Security Insight
> `--network host`를 사용하면 Docker화된 MCP 서버가 개발자 머신(localhost)에서 실행 중인 Postgres 인스턴스에 연결할 수 있습니다. 하지만 이 힘에는 위험이 따릅니다. 일반적인 작업에는 반드시 **읽기 전용 사용자(Read-only user)**로 구성된 `POSTGRES_URL`을 사용하여, 에이전트가 자율 루프 중에 `DROP TABLE`과 같은 파괴적인 쿼리를 우발적으로 실행하는 것을 방지해야 합니다.

### 3.4 "Shift Left" 보안: Snyk vs Trivy

MCP를 통해 보안 스캐너를 통합하면 에이전트가 "자가 치유(Self-healing)" 보안 워크플로우를 수행할 수 있습니다.

- **Snyk MCP**: 의존성 스캔 및 코드 분석에 최적화되어 있습니다. CLI 설치와 인증 토큰이 필요합니다.
- **Trivy MCP**: 컨테이너 및 파일 시스템 스캔에 최적화되어 있습니다. 가볍고 오픈 소스입니다.

**비교 및 선택**:
포괄적인 파이프라인을 위해서는 두 가지를 모두 오케스트레이션할 수 있습니다. Trivy는 컨테이너/인프라 계층을, Snyk는 애플리케이션 의존성 계층을 담당하게 하여, 에이전트가 스택의 여러 수준에서 보안 결함을 인지하는 **[[DevSecOps]]** 등의 "심층 방어(Defense in Depth)" 전략을 구축합니다.

---

## 4. 지식(Knowledge): 에이전트 스킬과 점진적 공개

### 4.1 컨텍스트 포화 문제

대규모 코드베이스에서 모든 문서, 스타일 가이드, 아키텍처 다이어그램을 에이전트에게 제공하는 것은 토큰 제한과 비용 문제로 불가능합니다. "컨텍스트 스터핑(Context Stuffing)"은 모델이 관련 지시 사항을 찾기 어렵게 만드는 "건초더미 속 바늘" 효과를 일으켜 성능을 저하시킵니다.

```mermaid
graph TD
    A[Initial State] -->|Load CLAUDE.md| B(Base Context)
    B -->|Task: DB Migration| C{Check Skills}
    C -->|Found| D[Load db-migration/SKILL.md]
    C -->|Not Found| E[Keep Context Light]
    D --> F(Expanded Context)
    style F fill:#f9f,stroke:#333
    style B fill:#ccf,stroke:#333
```

**에이전트 스킬(Agent Skills)**은 점진적 공개(Progressive Disclosure) 메커니즘을 통해 이를 해결합니다. 스킬은 필요할 때까지 휴면 상태로 유지되는 지식 묶음입니다.

### 4.2 SKILL.md의 해부학

스킬은 `SKILL.md` 파일을 포함하는 디렉토리로 정의됩니다.

**구조**:
- **YAML Frontmatter**: `name`과 `description`을 정의합니다. 설명은 "의미론적 트리거(Semantic Trigger)" 역할을 합니다.
- **지시 본문(Instruction Body)**: 해당 작업의 "표준 운영 절차(SOP)"입니다.
- **리소스(Resources)**: 스킬 디렉토리에 포함된 다른 파일이나 스크립트에 대한 참조입니다.

**예시: db-migration 스킬 (`.claude/skills/db-migration/SKILL.md`)**
```markdown
---
name: db-migration
description: 사용자가 데이터베이스 스키마 변경, 새 테이블 생성 또는 컬럼 수정을 요청할 때마다 이 스킬을 사용하십시오.
---
# 데이터베이스 마이그레이션 표준
당신은 데이터베이스 아키텍트입니다. 모든 스키마 변경에 대해 다음의 엄격한 프로토콜을 따르십시오:

1. **검증**: 코드를 작성하기 전에 `postgres` MCP 도구를 사용하여 대상 테이블의 현재 스키마를 검사하십시오.
2. **멱등성(Idempotency)**: 모든 SQL 마이그레이션은 멱등성을 가져야 합니다 (`IF NOT EXISTS` 사용).
3. **명명 규칙**: 파일 이름은 `YYYYMMDD_HHMM_description.sql` 형식을 따라야 합니다.
4. **롤백**: 반드시 해당하는 `down` 마이그레이션 파일을 생성해야 합니다.

**금지된 패턴**:
<bad-example>
CREATE TABLE users (id SERIAL PRIMARY KEY);
</bad-example>

**필수 패턴**:
<good-example>
CREATE TABLE IF NOT EXISTS users (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid()
);
</good-example>
```

---

## 5. 노동력: 서브 에이전트와 컨텍스트 격리

### 5.1 서브 에이전트 위임 이론

스킬이 지식을 제공한다면, **서브 에이전트(Sub-Agents)**는 전문화와 *컨텍스트 위생(Context Hygiene)*을 제공합니다. 서브 에이전트는 특정 시스템 프롬프트와 엄선된 도구 세트를 가진 Claude 모델의 별도 인스턴스입니다.

서브 에이전트의 주된 아키텍처적 이점은 **컨텍스트 격리(Context Isolation)**입니다. 긴 디버깅 세션에서 메인 스레드는 실패한 시도, 긴 오류 로그 등의 "잔해(Debris)"를 축적합니다. 서브 에이전트는 작업을 수행하고 최종 결과(또는 요약)만을 메인 에이전트에 반환하여 메인 컨텍스트를 깨끗하게 유지합니다.

### 5.2 서브 에이전트 구성

**구성 예시: "QA 엔지니어" (`.claude/agents/qa-engineer.md`)**
```markdown
---
name: qa-engineer
description: 변경 사항이 감지되면 선제적으로 테스트를 실행하고 코드 품질을 검증합니다.
tools: Bash, Read, Grep, Snyk-Scanner
model: opus
permissionMode: acceptEdits
---
당신은 냉소적인 품질 보증(QA) 엔지니어입니다...
 - 분석: `git diff`를 읽어 최근 변경 사항을 파악하십시오.
 - 테스트: `npm test`를 사용하여 관련 테스트 스위트를 실행하십시오.
 - 보고: 버그를 직접 수정하지 마십시오. 실패에 대한 구조화된 보고서를 출력하십시오.
```
* **`model: opus`**: 추론이 많이 필요한 작업에는 가장 성능이 좋은 모델을 강제로 사용합니다.
* **`permissionMode: acceptEdits`**: 사용자 확인 없이 읽기 전용 도구를 실행할 수 있게 하여 원활한 백그라운드 검증 루프를 생성합니다.

---

## 6. 반사 신경: 커스텀 슬래시 커맨드

### 6.1 프롬프트에서의 유닉스 철학

커스텀 슬래시 커맨드(`.claude/commands/`에 저장됨)는 Claude Code의 자동화 레이어입니다. 이는 "모든 프로그램의 출력이 다른 프로그램의 입력이 되게 하라"는 유닉스 철학을 따릅니다.

### 6.2 고급 커맨드 구문

- **인수(Arguments)**: `$1`, `$2` 또는 `$ARGUMENTS`를 통해 사용자 입력을 캡처.
- **Bash 보간(Interpolation)**: `!command`를 사용하여 쉘 명령어를 실행하고 그 출력을 프롬프트에 주입.
- **컨텍스트 참조**: `@file`을 사용하여 특정 파일을 강제로 포함.

**예시: /deep-review 커맨드**
```markdown
description: 스테이징된 변경 사항에 대해 심층적인 아키텍처 및 보안 검토를 수행합니다.
# 심층 검토 요청
현재 git에 스테이징된 변경 사항을 분석하십시오:
!git diff --name-only --cached

사용자는 다음 사항에 초점을 맞춘 검토를 요청하고 있습니다: $ARGUMENTS

**절차**:
1. `architect-agent` 서브 에이전트를 생성하십시오.
2. 서브 에이전트가 `CLAUDE.md`의 원칙에 따라 diff를 검토하게 하십시오.
3. `security-audit` 스킬의 로직을 사용하여 보안 취약점을 확인하십시오.
4. 위험 평가 점수(1-10)를 출력하십시오.
```

---

## 7. 통합 오케스트레이션 워크플로우

Claude Code의 진정한 힘은 MCP, 스킬, 서브 에이전트, 커맨드라는 네 가지 기둥이 응집력 있는 워크플로우로 엮일 때 드러납니다.

### 7.1 워크플로우 A: 명세 주도 개발 (SDD)

"계획 모드(Plan Mode)"(Shift+Tab)는 SDD의 초석입니다. 이는 *생각(Thinking)*과 *행동(Doing)*의 분리를 강제합니다.

1. **계획 단계 (아키텍트)**: 사용자가 "GraphQL 마이그레이션 계획"을 요청 -> Claude가 `PLAN.md` 생성. (Reading Only)
2. **검토 단계 (인간)**: 인간이 `PLAN.md` 승인 및 커밋.
3. **실행 단계 (건설자)**:
   - 사용자가 `/implement PLAN.md` 실행.
   - 커맨드 로직: `PLAN.md`의 각 섹션에 대해 서브 에이전트(`coding-subagent`)를 생성하여 구현하고, `qa-engineer`로 검증.

### 7.2 워크플로우 B: "Zero-Touch" 릴리스

이 워크플로우는 보안, 테스트, 문서화 도구를 조율하여 릴리스 프로세스를 자동화합니다.

**트리거**: `/release v1.2.0`

**시퀀스**:
1. **사전 점검**: `!git status`로 트리 확인.
2. **테스트 검증**: `qa-engineer` 호출 -> 테스트 수행.
3. **보안 게이트**: `trivy` MCP 호출 -> CVE 스캔. (실패 시 중단)
4. **변경 로그**: `github` MCP 호출 -> PR 목록 수집 및 요약 생성.
5. **태깅**: `git tag` 및 `git push`.
6. **공지**: `slack` MCP 호출 -> 배포 알림 전송.

---

## 8. 거버넌스, 보안 및 향후 전망

에이전트가 자율화될수록 보안 통제는 에이전트 구성으로 **"Shift Left(좌측 이동)"** 해야 합니다.

- **최소 권한 원칙**: `.claude/settings.json`을 사용하여 엄격한 `allowedTools`를 정의하십시오.
- **비밀 관리**: API 키를 절대 설정 파일에 저장하지 말고, `${VAR}` 환경 변수 확장을 사용하십시오.
- **Docker 격리**: 모든 타사 MCP 도구는 반드시 Docker 컨테이너에서 실행하십시오.

Claude Code의 오케스트레이션은 인간의 의도가 합성 지능에 의해 증폭되는 하이브리드 모델로서 소프트웨어 개발의 미래를 대변합니다. MCP(도구), 스킬(지식), 서브 에이전트(노동력), 커맨드(자동화)의 상호 작용을 마스터함으로써 엔지니어링 팀은 탄력적이고 자가 수정 가능한 개발 환경을 구축할 수 있습니다. 미래는 이러한 합성 인력을 효과적으로 조율할 수 있는 사람들의 것입니다.

---

## 9. 부록: 상세 구성 참조

### 부록 A: 전체 `.claude/mcp.json` 템플릿

```json
{
  "mcpServers": {
    "github": {
      "command": "docker",
      "args": ["run", "-i", "--rm", "-e", "GITHUB_PERSONAL_ACCESS_TOKEN", "mcp/github"],
      "env": { "GITHUB_PERSONAL_ACCESS_TOKEN": "${GITHUB_TOKEN}" }
    },
    "postgres": {
      "command": "docker",
      "args": ["run", "-i", "--rm", "-e", "POSTGRES_URL", "--network", "host", "mcp/postgres"],
      "env": { "POSTGRES_URL": "postgres://user:pass@localhost:5432/db" }
    },
    "snyk": {
      "command": "npx",
      "args": ["-y", "snyk", "mcp", "-t", "stdio"],
      "env": { "SNYK_TOKEN": "${SNYK_TOKEN}" }
    }
  }
}
```

### 부록 B: `CLAUDE.md` "헌법" 템플릿

```markdown
# 프로젝트: E-Commerce Monorepo

## 기술 스택
- **프론트엔드**: Next.js (App Router)
- **백엔드**: NestJS
- **DB**: Postgres

## 핵심 지침
- **아키텍처**: `src/core` 내의 헥사고날 아키텍처 패턴을 따를 것.
- **테스트**: 테스트 없는 PR 금지. `qa-engineer` 에이전트를 사용하여 검증할 것.
- **보안**: 모든 의존성은 Snyk 스캔을 통과해야 함.

## 워크플로우 포인터
- **DB 변경 시**: `skill:db-migration` 사용.
- **API 변경 시**: `skill:secure-api-endpoint` 사용.
- **배포 시**: `/release` 사용.
```

### 부록 C: 일반적인 문제 해결 (Troubleshooting)

| 문제                   | 증상                                     | 근본 원인                                                     | 해결책                                                                  |
| :--------------------- | :--------------------------------------- | :------------------------------------------------------------ | :---------------------------------------------------------------------- |
| **MCP 연결 거부**      | "Error connecting to MCP server"         | Docker 컨테이너가 localhost를 볼 수 없음.                     | `mcp.json`의 Docker 실행 인수에 `--network host` 추가.                  |
| **컨텍스트 기억 상실** | 에이전트가 `CLAUDE.md` 지시를 무시함.    | 컨텍스트 윈도우 포화.                                         | `/compact`를 실행하거나 `CLAUDE.md`를 더 작은 `SKILL.md` 파일들로 분할. |
| **서브 에이전트 환각** | 서브 에이전트가 없는 도구를 실행하려 함. | 에이전트 설정의 `tools` 목록이 너무 관대하거나 정의되지 않음. | 에이전트 정의에 `tools:`를 명시적으로 정의.                             |
| **권한 피로**          | "Allow this?" 프롬프트가 계속 뜸.        | 기본 권한 모드가 너무 엄격함.                                 | 읽기 전용 도구에 대해 `.claude/settings.json`에서 `allowedTools` 구성.  |