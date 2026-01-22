---
tags: [git, github, guide, version_control]
Up: [[Tech_Stack_MOC]]
---

# Git + GitHub 실전 가이드 (2026 기준)

**2026년 기준**으로 최신(2025~2026) Git + GitHub 완전 초보자 → 실무 입문 수준까지 커버하는 실전 중심 가이드입니다.

GitHub Desktop이나 VS Code Source Control을 쓰는 것도 좋지만, **장기적으로는 CLI(터미널)를 익히는 걸 강력 추천**합니다.

## 1. Git vs GitHub 한 줄 정리 (매우 중요!)

| 항목      | Git                                | GitHub                                         |
| --------- | ---------------------------------- | ---------------------------------------------- |
| 무엇인가  | **버전 관리 도구** (프로그램)      | **Git 저장소를 보관·공유하는 클라우드 서비스** |
| 설치 장소 | 내 컴퓨터에 설치                   | 웹사이트 github.com                            |
| 주요 용도 | 로컬에서 커밋·브랜치·히스토리 관리 | 원격 저장소(remote), 협업, PR, 이슈 등         |
| 필수 여부 | 거의 모든 개발자 필수              | 개인/팀 프로젝트 거의 필수                     |

## 2. 설치 & 최초 설정 (꼭 한 번만!)

**Windows / Mac / Linux 공통**

1. Git 설치  
   [git-scm.com/downloads](https://git-scm.com/downloads) → 최신 버전 다운로드 & 설치

2. 터미널(Windows → Git Bash 또는 PowerShell) 열고 설정

```bash
# 이름 & 이메일 (GitHub 가입 이메일과 동일하게!)
git config --global user.name "JongMan Park"
git config --global user.email "your_real@email.com"

# 기본 브랜치 이름 main으로 (2020년 이후 표준)
git config --global init.defaultBranch main

# 편한 에디터 설정 (선택)
git config --global core.editor "code --wait"   # VS Code
# 또는 nano, vim 등

# 설정 확인
git config --list
```

3. GitHub 계정 만들기 (이미 있다면 생략)  
   → [github.com](https://github.com) → Sign up

4. **2025~2026 추천 인증 방식**: Personal Access Token(PAT) 또는 GitHub CLI 사용  
   (비밀번호 방식은 거의 막힘)

## 3. 가장 많이 쓰는 10가지 명령어 패턴 (90% 이상 커버)

| 순서 | 할 일                           | 명령어 패턴 (자주 쓰는 순)                                     | 설명                        |
| ---- | ------------------------------- | -------------------------------------------------------------- | --------------------------- |
| 1    | 새 프로젝트 시작                | `git init`                                                     | 폴더를 git 저장소로 만듦    |
| 2    | 상태 확인 (제일 많이 씀!)       | `git status` 또는 `gst` (alias)                                | 무엇이 바뀌었는지 항상 확인 |
| 3    | 파일 → staging area             | `git add 파일명` <br>`git add .` (전체)                        | 커밋할 준비                 |
| 4    | 스냅샷 저장 (커밋)              | `git commit -m "기능: 회원가입 API 추가"`                      | 의미 있는 메시지 필수!      |
| 5    | 원격 저장소 연결 (처음 1번)     | `git remote add origin https://github.com/아이디/저장소명.git` | —                           |
| 6    | GitHub로 업로드                 | `git push -u origin main` (최초) <br>`git push` (이후)         | —                           |
| 7    | GitHub → 내 컴퓨터 동기화       | `git pull`                                                     | 충돌 날 수 있음             |
| 8    | 새 기능 개발 시작               | `git checkout -b feature/로그인페이지`                         | 브랜치 생성+이동            |
| 9    | 브랜치 → 메인 병합 (GitHub에서) | Pull Request 생성 → Review → Merge                             | CLI로는 `git merge`         |
| 10   | 실수 취소하고 싶을 때           | `git restore 파일명`<br>`git reset --hard HEAD~1` (위험)       | 조심해서 사용               |

## 4. 실전 추천 워크플로우 (2025~2026 표준)

### 개인 프로젝트

```bash
# 1. GitHub에서 먼저 저장소 생성 (Add README, .gitignore 선택)

# 2. 로컬에서
git clone https://github.com/아이디/저장소명.git
cd 저장소명

# 3. 이제부터 반복
(코드 수정)
git status
git add .
git commit -m "fix: 버튼 색상 다크모드 지원"
git push
```

### 팀/회사 프로젝트 (GitHub Flow)

1. `main` 브랜치는 배포 가능한 상태만 유지
2. 기능/버그마다 브랜치 생성  
   `git switch -c feature/결제-취소-api`
3. 열심히 개발 → `add → commit → push`
4. GitHub에서 **Pull Request** 생성
5. 코드 리뷰 받음 → Approve → Squash & Merge 또는 Merge commit
6. 로컬에서 동기화  
   `git switch main`  
   `git pull`

## 5. 초보자가 자주 하는 실수 TOP 7 + 해결법

| 실수                    | 증상                         | 해결법 (빠른 순)                                      |
| ----------------------- | ---------------------------- | ----------------------------------------------------- |
| force push 남발         | 히스토리 날아감              | `--force-with-lease` 쓰기                             |
| main에 직접 커밋        | PR 리뷰 불가                 | 브랜치 만들어서 작업                                  |
| .env, node_modules 커밋 | 토큰 유출 / 저장소 용량 폭발 | `.gitignore`에 추가                                   |
| pull 전에 push          | rejected non-fast-forward    | `git pull --rebase` 후 다시 push                      |
| 커밋 메시지 대충        | 나중에 후회                  | Conventional Commits 규칙 추천                        |
| 충돌났을 때 당황        | —                            | `git status` → conflict 파일 직접 수정 → add → commit |
| token / ssh 설정 안 함  | 계속 아이디/비번 물어봄      | GitHub CLI 설치 → `gh auth login` 추천                |

## 6. 2026년 추천 추가 설정 (시간 아끼기)

```bash
# 자주 쓰는 alias (~/.gitconfig 또는 .bashrc/.zshrc)
[alias]
    st = status
    co = checkout
    br = branch
    ci = commit
    cm = commit -m
    lg = log --oneline --graph --decorate --all
    undo = reset --soft HEAD~1
    last = log -1 HEAD
```

```bash
# .gitignore 전용 사이트 (꼭!)
https://www.toptal.com/developers/gitignore
```

## 한 줄 요약 로드맵

1. Git 설치 → 이름/이메일 설정
2. GitHub 계정 만들기
3. 저장소 1개 만들어 clone 해보기
4. 파일 수정 → add → commit → push 연습 5번
5. 브랜치 만들어서 Pull Request까지 올려보기
6. 팀원이랑 협업 1번 해보기

👉 **다음 단계**: [[GitHub_Advanced_Features_2026|GitHub 고급 기능 실무 가이드 (PR, Actions, Worktree 등)]]
