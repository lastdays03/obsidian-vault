---
sources:
  - User Chat
tags:
  - knowledge/tech_stack/dev_tools
  - tmux
  - terminal
---

# tmux (Terminal Multiplexer)

## 1. Definition (정의)
**tmux**는 **Terminal Multiplexer**의 약자로, 단일 터미널 창 내에서 여러 세션, 윈도우, 패널을 생성하고 관리할 수 있게 해주는 도구입니다. 
가장 강력한 기능은 **세션 유지(Persist)**로, SSH 연결이 끊기거나 터미널을 닫아도 백그라운드에서 프로세스가 계속 실행되어 서버 작업 시 필수적입니다.

## 2. Core Concepts (핵심 개념)
구조: `Session` > `Window` > `Pane`

1. **Session (세션)**
    - 프로젝트 단위의 작업 공간 (예: `backend`, `infra`, `monitoring`).
    - **가장 상위 개념**으로, 데몬 서버에서 관리되므로 연결이 끊겨도 살아있습니다.
2. **Window (윈도우)**
    - 브라우저의 **탭(Tab)**과 같은 개념.
    - 하나의 세션 내에서 여러 개의 전체 화면을 전환하며 작업할 수 있습니다.
3. **Pane (패널)**
    - 하나의 윈도우를 **분할(Split)**한 화면.
    - 좌우/상하로 나누어 로그 확인, 코드 편집, 서버 실행을 동시에 할 수 있습니다.

## 3. Key Commands (주요 명령어)
**(Prefix Key: `Ctrl + b`)** - 모든 단축키는 이 키를 먼저 누르고 뗀 뒤 입력합니다.

| 기능            | 명령어/단축키                 | 설명                                |
| :-------------- | :---------------------------- | :---------------------------------- |
| **세션 관리**   | `tmux new -s <이름>`          | 새 세션 생성                        |
|                 | `tmux attach -t <이름>`       | 기존 세션으로 복귀 (Attach)         |
|                 | `tmux ls`                     | 실행 중인 세션 목록 확인            |
|                 | `tmux kill-session -t <이름>` | 세션 종료                           |
| **윈도우 (탭)** | `Prefix` + `c`                | 새 창 생성 (Create)                 |
|                 | `Prefix` + `n` / `p`          | 다음(Next) / 이전(Previous) 창 이동 |
|                 | `Prefix` + `<숫자>`           | 해당 번호의 창으로 이동             |
| **패널 (분할)** | `Prefix` + `"`                | **가로** 분할                       |
|                 | `Prefix` + `%`                | **세로** 분할                       |
|                 | `Prefix` + `방향키`           | 패널 간 포커스 이동                 |
|                 | `Prefix` + `z`                | 현재 패널 전체화면 토글 (Zoom)      |

## 4. Problem & Solution (문제 해결)

### A. 마우스 스크롤이 안 되는 문제
tmux는 기본적으로 마우스 지원(휠 스크롤, 클릭)이 꺼져 있어, 스크롤을 하면 명령어 히스토리가 나오거나 아무 반응이 없습니다.

- **해결 1 (설정 적용)**: `~/.tmux.conf` 파일에 `set -g mouse on` 추가 후 적용.
    ```bash
    # 설정 파일 적용
    tmux source-file ~/.tmux.conf
    ```
- **해결 2 (일회성)**: tmux 실행 상태에서 `Prefix` + `:` 누른 뒤 `set -g mouse on` 입력.
- **대안 (키보드)**: `Prefix` + `[` 를 눌러 **Copy Mode**로 진입하면 방향키나 PageUp/Down으로 스크롤 가능. (`q` 또는 `Enter`로 종료)

### B. 로그가 잘려서 안 보임 (History Limit)
기본 히스토리 라인 수가 적어서 긴 로그는 위쪽 내용이 사라집니다.

- **해결**: `~/.tmux.conf`에 `set -g history-limit 100000` 설정 추가.

### C. macOS iTerm2 충돌
iTerm2 사용 시 마우스 휠이 제대로 작동하지 않는 경우.

- **설정 확인**: iTerm2 > Preferences > Profiles > Terminal
    - **"Scroll wheel sends arrow keys"** 체크 **해제** 필요.
    - **"Alternate screen scroll"** 옵션 확인.
- **TERM 설정**: `echo $TERM` 확인 후 `screen-256color` 권장.

## 5. Configuration (추천 설정)
`~/.tmux.conf` 파일에 아래 내용을 저장하여 사용성을 높일 수 있습니다.

```bash
# 1. 마우스 사용 (스크롤, 리사이즈, 패널 선택 가능)
set -g mouse on

# 2. 스크롤 히스토리 넉넉하게 (기본값은 너무 작음)
set -g history-limit 100000

# 3. 색상 설정 (vim 등에서 색상 깨짐 방지)
set -g default-terminal "screen-256color"

# 4. 인덱스 설정 (0번은 키보드에서 너무 머니까 1번부터 시작)
set -g base-index 1
setw -g pane-base-index 1

# 5. 윈도우 닫으면 번호 자동 정렬 (1, 3번 남으면 1, 2번으로 정렬)
set -g renumber-windows on
```

## 6. Practical Layout (실전 활용)
서버 디버깅 시 추천 레이아웃 (4분할):

```text
┌─────────────────────────┬─────────────────────────┐
│ [1] Editor / AI Tool    │ [2] Test Runner         │
│ (Claude CLI, Vim)       │ (pytest, mvn test)      │
├─────────────────────────┼─────────────────────────┤
│ [3] Git Status / Diff   │ [4] Log Watcher         │
│                         │ (docker logs -f)        │
└─────────────────────────┴─────────────────────────┘
```

## 7. References
- User Chat Provide (2026-02-13)
