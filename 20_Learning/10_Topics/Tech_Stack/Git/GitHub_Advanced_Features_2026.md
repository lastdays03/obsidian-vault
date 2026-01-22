---
tags: [git, github, advanced, collaboration, ci_cd]
Up: [[Git_GitHub_Guide_2026]]
---

# GitHub 고급 기능 실무 가이드 (2026 기준)

이 문서는 **Git + GitHub**에서 자주 혼동되거나 실무에서 필수적으로 사용되는 **5대 핵심 기능 (Pull Request, Actions, Projects, Wiki, Worktree)**에 대한 **2026년 기준 실무 중심 정리**입니다.

| 기능                  | 어디에 속함?           | 주 용도 (한 줄)                                         | 협업 수준 | 실제 쓰임새 예시 (2025~2026 트렌드)                              | 난이도 | 추천 학습 순서  |
| --------------------- | ---------------------- | ------------------------------------------------------- | --------- | ---------------------------------------------------------------- | ------ | --------------- |
| **Pull Request (PR)** | GitHub (웹)            | 코드 변경 제안 → 리뷰 → 병합                            | ★★★★★     | 거의 모든 팀/오픈소스 필수, Conventional PR + Auto-merge         | ★★☆☆☆  | 1위 (제일 먼저) |
| **GitHub Actions**    | GitHub (CI/CD)         | 자동화 (빌드·테스트·배포·라벨링·PR 자동화 등)           | ★★★★☆     | PR마다 lint/test/build, release 자동 생성, dependabot 자동 merge | ★★★☆☆  | 2~3위           |
| **GitHub Projects**   | GitHub (프로젝트 관리) | 칸반/테이블/로드맵 형태 이슈·PR 관리 보드               | ★★★★☆     | 스프린트 관리, 로드맵 공개, 자동화 필드 (status, iteration)      | ★★☆☆☆  | 3~4위           |
| **Wiki**              | GitHub (문서)          | 프로젝트 설명서·가이드·API 문서 등 자유 문서            | ★★☆☆☆     | 오픈소스 README 다음으로 많이 보는 곳, Side project 문서화       | ★☆☆☆☆  | 언제든          |
| **git worktree**      | Git (로컬 CLI)         | 같은 저장소에서 **여러 브랜치 동시 작업** 디렉토리 생성 | ★★★☆☆     | PR 리뷰용 임시 디렉토리, hotfix + feature 동시 작업              | ★★★★☆  | 중급 이후       |

## 1. Pull Request (PR) — 협업의 핵심

GitHub에서 가장 중요한 기능 중 하나로, 팀 프로젝트의 심장입니다.

**주요 흐름 (2026년 표준)**

```text
feature/xxx 브랜치에서 개발
→ git push
→ GitHub에서 Compare & pull request 클릭
→ 제목/본문 작성 (Conventional Commits 추천)
→ Reviewers 지정 + Assignees + Labels + Projects 연결
→ Draft PR → Ready for review
→ Code review (댓글, suggestion, approval)
→ Checks (Actions) 통과
→ Squash & merge / Rebase & merge / Merge commit
```

**2026 트렌드**

- **Draft PR 기본 사용**: 작업 중임을 알리고 피드백을 빠르게 받기 위함
- **Required reviews + CODEOWNERS**: 핵심 코드 보호 및 책임자 지정 (필수)
- **Auto-merge**: 승인 + CI(테스트) 통과 시 자동으로 병합되도록 설정
- **PR 템플릿 + Issue 연결**: `#123` 등의 키워드로 이슈 자동 닫기

## 2. GitHub Actions — 자동화 엔진

`.github/workflows/*.yml` 파일로 정의하며, CI/CD의 표준이 되었습니다.

**가장 많이 쓰는 트리거 예시**

```yaml
on:
  pull_request:
    branches: [main]
  push:
    branches: [main]
  issues:
    types: [opened, labeled]
  workflow_dispatch:    # 수동 실행 버튼
```

**실무 Top 5 Actions 패턴 (2026)**

1. **CI Pipeline**: PR마다 ESLint + Prettier + Unit Test 실행
2. **Dependabot**: 의존성 업데이트 PR 자동 병합 (테스트 통과 시)
3. **Release Automation**: 태그 생성 시 Changelog 자동 생성 + GitHub Release 발행
4. **Project Automation**: Issue/PR 생성 시 GitHub Projects 보드에 자동 추가
5. **Stale Bot**: 오래 방치된 이슈에 자동으로 라벨을 붙이거나 닫음

## 3. GitHub Projects (vNext)

2023~2025년에 v2로 완전히 대세가 되었으며, JIRA/Trello를 대체하고 있습니다.

**주요 필드 종류**

- **Status**: Todo → In Progress → Done
- **Iteration**: 스프린트 관리 (2주 단위 등)
- **Custom Fields**: Date, Number, Single select, Labels
- **Rollup**: 하위 작업 진행률 자동 계산

**강력한 자동화 예시**

- PR이 Merge되면 Status를 자동으로 `Done`으로 변경
- `bug` 라벨이 붙으면 Priority를 자동으로 `High`로 설정
- Milestone 연결 시 현재 Iteration 자동 할당

## 4. Wiki — 간단 문서 공간

**특징 (2026년 기준)**

- 메인 저장소와 별도의 git 저장소 (`{repo}.wiki.git`)로 관리됩니다.
- **Pull Request 지원 안 됨**: 2026년에도 여전히 Native PR을 지원하지 않아 협업 검수에는 불편할 수 있습니다.
- **대안**: 저장소 내 `docs/` 폴더 + MkDocs/VitePress + GitHub Actions로 배포하는 방식을 더 선호합니다.

**실무 선택지 2026**

- **Wiki**: 아주 간단한 프로젝트, 개인 메모, 빠른 가이드
- **Docs as Code**: `docs/` 폴더 관리 (PR 가능, 버전 관리 용이)
- **External**: Confluence / Notion (기업용)

## 5. git worktree — 로컬 멀티태스킹의 신

`git clone`을 여러 번 하지 않고, 하나의 `.git` 데이터베이스를 공유하면서 여러 작업 디렉토리(Worktree)를 동시에 가질 수 있는 **고급 기능**입니다.

**가장 유용한 3가지 패턴**

```bash
# 1. PR 리뷰용 임시 디렉토리 (가장 많이 씀)
# 현재 작업 중인 브랜치를 끄지 않고, 남의 PR을 받아와서 실행해볼 때
git fetch origin pull/123/head:pr-123
git worktree add ../review-pr-123 pr-123

# 2. feature + hotfix 동시에
# 긴 기능 개발 중에 긴급 버그 수정이 들어왔을 때
git worktree add ../hotfix-urgent origin/main
cd ../hotfix-urgent
git checkout -b hotfix/critical-bug

# 3. release 브랜치 미리 확인
git worktree add ../release-next release/next
```

**주의사항**

- **Checkout 제한**: 이미 다른 worktree에서 체크아웃된 브랜치는 동시에 체크아웃할 수 없습니다.
- **관리**: `git worktree list`로 목록 확인, `git worktree remove`로 디렉토리 삭제 (단순 `rm -rf`보다 명령어 삭제 권장)
- **의존성 설치**: `.git`만 공유하므로 `node_modules` 등은 각 worktree 폴더마다 새로 `npm install` 해야 합니다.

## 한눈에 보는 로드맵 (초보 → 실무)

| 단계        | 집중할 것                                            |
| :---------- | :--------------------------------------------------- |
| **1~2주**   | git 기본 + Pull Request 10번 올려보기                |
| **3~4주**   | GitHub Actions 기본 workflow 만들기 (test + lint)    |
| **1~3개월** | GitHub Projects로 개인/팀 일정 관리 시작             |
| **3개월~**  | git worktree + rebase + interactive rebase 익히기    |
| **6개월~**  | Wiki 대신 docs/ + GitHub Pages + Actions sync 자동화 |
