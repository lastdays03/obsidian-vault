---
tags:
  - knowledge/topic
  - tech_stack/claude
  - system_configuration
  - operations_manual
source: "User Standard v1.0"
Up: [[Claude_Code_MOC]]
---

# Claude Code 표준 셋업 가이드 v1.0
*(Claude Code Standard Setup Guide v1.0)*

> **문서 분류**: 기술 구성 가이드 / 운영 표준  
> **대상 독자**: Claude Code를 도입하려는 DevOps 엔지니어  
> **목적**: 안전하고 효율적인 에이전트 개발 환경 구성을 위한 표준 디렉토리 구조 및 설정 파일 템플릿 제공

---

## 1. 개요 및 표준 구조

이 가이드는 Claude Code를 단순한 CLI 도구가 아닌, **MCP(도구), 스킬(지식), 에이전트(노동력)**가 결합된 체계적인 시스템으로 운영하기 위한 표준 구성을 정의합니다. 이를 통해 팀 단위의 일관된 개발 환경을 보장합니다.

### 1.1 표준 디렉토리 구조 (The Antigravity File Tree)

모든 Antigravity 프로젝트는 다음의 파일 구조를 준수해야 합니다.

```bash
PROJECT_ROOT/
├── .claude/
│   ├── settings.json       # 프로젝트 거버넌스 (허용된 도구, 권한)
│   ├── mcp.json            # 도구 연결 설정 (Docker 컨테이너 정의)
│   ├── skills/             # [지식 저장소]
│   │   ├── db-migration/
│   │   │   └── SKILL.md
│   │   └── secure-api/
│   │       └── SKILL.md
│   ├── agents/             # [인력 사무소]
│   │   ├── qa-engineer.md
│   │   └── security-auditor.md
│   └── commands/           # [자동화 매크로]
│       ├── implement.md
│       └── release.md
├── CLAUDE.md               # [헌법] 프로젝트 기술 스택 및 대원칙
└── .env                    # [비밀] API 키 및 로컬 환경 변수 (Git 제외 필수)
```

### 1.2 핵심 구성 파일: `.claude/settings.json`

이 파일은 에이전트의 "물리적 한계"를 정의합니다. 보안 사고 방지를 위해 `allowedTools`를 명시적으로 제어하십시오.

```json
{
  "commands": {
    "allowedTools": [
      "Bash",
      "Grep",
      "GlobTool",
      "Read",
      "Edit",       // 주의: 메인 에이전트에게만 허용 권장
      "LSP"
    ]
  },
  "autoApproval": {
    "enabled": true,
    "maxRequests": 20 // 무한 루프 방지용 안전 장치
  }
}
```

---

## 2. MCP 아키텍처 구축 (The Trinity Architecture)

Antigravity 시스템은 **소스(GitHub), 데이터(Postgres), 보안(Snyk/Trivy)**의 3대 요소(Trinity)를 MCP로 연결하여 작동합니다. 모든 MCP 서버는 Docker로 격리되어야 합니다.

### 2.1 MCP 설정 표준 (`.claude/mcp.json`)

아래 코드를 복사하여 설정하되, 비밀 키는 반드시 환경 변수(`${VAR}`)로 주입하십시오.

```json
{
  "mcpServers": {
    "github": {
      "command": "docker",
      "args": ["run", "-i", "--rm", "-e", "GITHUB_PERSONAL_ACCESS_TOKEN", "mcp/github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${GITHUB_TOKEN}"
      }
    },
    "postgres": {
      "command": "docker",
      "args": [
        "run", "-i", "--rm", 
        "-e", "POSTGRES_URL", 
        "--network", "host",  // 호스트 DB 접근을 위해 필수
        "mcp/postgres"
      ],
      "env": {
        "POSTGRES_URL": "postgres://developer:dev_password@localhost:5432/project_db"
      }
    },
    "trivy": {
      "command": "docker",
      "args": [
        "run", "-i", "--rm",
        "-v", "/var/run/docker.sock:/var/run/docker.sock", // Docker 스캔을 위한 볼륨 마운트
        "aquasec/trivy", "server", "--listen", "0.0.0.0:4954"
      ]
    }
  }
}
```

> [!WARNING] 운영 주의사항
> - **DB 연결**: `POSTGRES_URL`에 사용되는 계정은 스키마 변경이 필요한 경우를 제외하고는 기본적으로 DDL 권한이 제한된 계정을 사용하는 것이 안전합니다.
> - **도커 네트워크**: 로컬 호스트의 DB에 접근하려면 `--network host` 옵션이 필수적입니다 (macOS/Linux).

---

## 3. 스킬 및 에이전트 정의 (Skill & Agent Definition)

지식과 노동력을 파일로 코드화(Codify)하여 재사용 가능한 자산으로 만듭니다.

### 3.1 스킬 정의 템플릿 (Knowledge Unit)

스킬은 에이전트가 수행해야 할 작업의 "표준 운영 절차(SOP)"입니다.

- **파일 위치**: `.claude/skills/[skill-name]/SKILL.md`

**작성 표준**:
```markdown
---
name: [동사]-[명사] (예: migrate-db)
description: [트리거 문구] 사용자가 DB 스키마 변경을 요청할 때 이 스킬을 로드하십시오.
---
# [스킬 이름] 표준

## [역할 정의]
당신은 수석 DBA입니다.

## [절차]
1. 현재 스키마 확인: `postgres` 도구 사용.
2. 마이그레이션 파일 생성: `YYYYMMDD_name.sql` 형식 준수.
3. 검증: `down` 스크립트가 존재하는지 확인.

## [금지 사항]
- `DROP TABLE` 사용 시 사용자에게 명시적 승인 요청 필수.
```

### 3.2 서브 에이전트 정의 템플릿 (Labor Unit)

서브 에이전트는 특정 도구 접근 권한과 페르소나를 가진 격리된 작업자입니다.

- **파일 위치**: `.claude/agents/[agent-role].md`

**작성 표준**:
```markdown
---
name: [agent-role] (예: qa-engineer)
description: 코드 변경 후 테스트 및 품질 검증을 수행합니다.
model: claude-3-5-sonnet-20240620
tools: [Bash, Read, Grep] # 편집 도구(Edit) 제외로 안전장치 마련
permissionMode: acceptEdits # 자동 실행 허용
---
# QA 엔지니어 지침

당신의 목표는 코드를 깨뜨리는 것입니다.
1. `git diff`를 분석하여 변경 범위를 파악하십시오.
2. 관련 단위 테스트를 실행하십시오 (`npm test --related`).
3. 실패 시 로그를 분석하여 원인을 요약 보고하십시오 (직접 수정하지 마십시오).
```

---

## 4. 개발자 루프 (The Antigravity Loop)

Antigravity 환경에서 개발자는 코드를 타이핑하는 것이 아니라, 의도를 주입하고 결과를 검증합니다. 다음은 표준 개발 사이클입니다.

### 4.1 1단계: 계획 (Plan Mode)
- **Action**: `Shift + Tab`을 눌러 계획 모드 진입.
- **Input**: "사용자 로그인 API에 Rate Limiting 기능을 추가해줘."
- **Process**: 시스템은 `CLAUDE.md`와 아키텍처 문서를 참조하여 구현 계획을 수립.
- **Output**: `PLAN.md` 파일 생성 (코드는 아직 작성되지 않음).

### 4.2 2단계: 구현 (Implementation)
- **Action**: `/implement` 커맨드 실행.
- **Command Logic** (`.claude/commands/implement.md`):

```markdown
---
description: PLAN.md 파일을 읽고 순차적으로 구현을 수행합니다.
---
# 구현 프로토콜

1. `@PLAN.md` 파일을 읽으십시오.
2. 계획의 각 단계(Step)에 대해:
    a. `coding-agent`를 호출하여 코드를 작성하십시오.
    b. 작성 후 `qa-engineer` 에이전트를 호출하여 테스트를 수행하십시오.
3. 모든 단계가 완료되면 요약 리포트를 출력하십시오.
```

### 4.3 3단계: 검증 및 릴리스 (Verify & Release)
- **Action**: `/release [version]` 커맨드 실행.
- **Process**:
    1. `trivy` MCP로 보안 스캔 수행.
    2. 전체 테스트 슈트 통과 확인.
    3. `github` MCP로 PR 생성 및 변경 로그 작성.

---

## 5. 문제 해결 (Troubleshooting)

시스템 운영 중 발생하는 일반적인 문제에 대한 대응 프로토콜입니다.

### 5.1 컨텍스트 고갈 (Context Exhaustion)
- **증상**: 에이전트가 느려지거나 이전 지시 사항(헌법)을 잊어버림.
- **원인**: 긴 터미널 로그와 파일 읽기로 컨텍스트 윈도우(200k 토큰) 포화.
- **해결책**:
    - `/compact` 명령어를 수동으로 실행하여 히스토리 압축.
    - `ccusage` (npm 패키지)를 설치하여 실시간 토큰 비용 모니터링.
    - 커스텀 커맨드 정의 시 중간중간 `/compact` 단계를 명시적으로 포함.

### 5.2 MCP 연결 실패 (Docker Networking)
- **증상**: "Connection refused" 또는 "Cannot connect to Postgres".
- **원인**: Docker 컨테이너 격리로 인해 로컬호스트(127.0.0.1)가 컨테이너 내부를 가리킴.
- **해결책**:
    - **Mac/Linux**: `mcp.json`의 `docker run` 명령어에 `--network host` 플래그 필수.
    - **Windows**: `host.docker.internal` DNS 사용.

### 5.3 무한 도구 루프 (Tool Loops)
- **증상**: 에이전트가 `ls`나 `grep`을 반복적으로 실행하며 멈추지 않음.
- **원인**: 에이전트가 원하는 정보를 찾지 못해 계속 탐색을 시도.
- **해결책**:
    - `Ctrl + C`로 즉시 중단.
    - 프롬프트에 "만약 3번 시도해도 찾지 못하면 멈추고 인간에게 물어봐"라는 제약 조건 추가.
    - `.claude/settings.json`의 `autoApproval.maxRequests` 값을 낮춤.
