---
tags:
  - Python
  - DevTools
  - uv
  - PackageManagement
created: 2026-01-16
updated: 2026-01-16
---

# uv: The Ultimate Python Package Manager

**uv**는 Astral에서 개발한 Rust 기반의 Python 패키지/환경 관리자입니다. 2026년 현재 Python 생태계에서 **가장 빠르고 편리한 올인원 툴**로 평가받으며, `pyenv`, `poetry`, `pip`, `pipx`, `virtualenv`의 기능을 단일 바이너리로 통합했습니다.

> [!TIP] **Why uv?**
> *   **Speed**: `pip` 대비 10~100배 빠른 설치 속도.
> *   **Unified**: Python 버전 관리부터 가상환경, 패키지 관리, 도구 실행까지 하나로 해결.
> *   **Reproducibility**: `uv.lock`을 통해 완벽한 의존성 재현 보장.

---

## 1. Installation

가장 추천하는 설치 방법 3가지입니다.

| 방법           | 명령어                                             | 추천 대상             | 비고               |
| :------------- | :------------------------------------------------- | :-------------------- | :----------------- |
| **Standalone** | `curl -LsSf https://astral.sh/uv/install.sh \| sh` | **Most Users** (권장) | 가장 빠르고 깨끗함 |
| **pipx**       | `pipx install uv`                                  | pipx 사용자           | pipx 필요          |
| **Homebrew**   | `brew install uv`                                  | macOS 사용자          | 편리한 업데이트    |

### Post-Installation Check
```bash
uv --version
# Output: uv 0.6.15 (or higher)
```
### Update
```bash
uv self update
```

---

## 2. Core Commands

### Project Initialization & Workflow
가장 자주 사용하는 패턴입니다.

```bash
# 1. Initialize Project
mkdir my-app && cd my-app
uv init                     # pyproject.toml 생성

# 2. Pin Python Version
uv python pin 3.13          # .python-version 파일 생성 (없으면 자동 다운로드)

# 3. Add Dependencies
uv add fastapi uvicorn      # 의존성 추가
uv add pytest ruff --dev    # 개발용 의존성 추가

# 4. Sync & Run
uv sync                     # uv.lock 생성 및 가상환경(.venv) 설치
uv run main.py              # 가상환경 자동 로드 후 실행
```

### Package Management
```bash
uv add requests             # 패키지 추가
uv remove pandas            # 패키지 제거
uv sync                     # Lock 파일 기준 동기화 (Clean Install)
uv pip install numpy        # (비권장) pyproject.toml 없이 pip처럼 설치
```

### Virtual Environment (Low-level)
`uv run` 사용 시 대부분 불필요하지만, 수동 제어가 필요할 때 사용합니다.
```bash
uv venv                     # 현재 경로에 .venv 생성
source .venv/bin/activate   # 수동 활성화
```

### Tool Execution (Replacement for pipx)
```bash
uv tool install black       # 글로벌 도구 설치
uv tool run black .         # 도구 실행 (단축: uvx black .)
```

### Python Version Management (Replacement for pyenv)
```bash
uv python list              # 설치 가능한 Python 버전 확인
uv python install 3.12      # 특정 버전 설치
uv python uninstall 3.10    # 삭제
```

### Cache Management
```bash
uv cache clean              # 캐시 삭제 (디스크 확보)
uv cache prune              # 미사용 캐시 정리
```

---

## 3. Recommended Workflows

### Scenario A: Starting a New FastAPI Project
```bash
mkdir super-api && cd super-api
uv init
uv python pin 3.13

# Core Dependencies
uv add "fastapi[standard]" uvicorn sqlalchemy pydantic-settings python-dotenv

# Dev Tools
uv add pytest httpx pytest-asyncio --dev

# Go!
uv sync
uv run uvicorn main:app --reload
```

### Scenario B: Migrating Legacy Project
```bash
cd existing-project
uv init                     # pyproject.toml 생성
uv python pin 3.12
uv add -r requirements.txt  # 기존 requirements.txt 의존성 가져오기
uv sync
```

---

## 4. Summary

**uv**는 초기 10분의 학습 투자로 기존의 복잡한 Python 툴체인(`pyenv` + `poetry` + `pipx`)을 완전히 대체할 수 있습니다. 

*   **속도**: 캐싱과 Rust 기반 병렬 처리로 압도적인 퍼포먼스.
*   **디스크 효율**: 하드링크를 사용한 글로벌 캐싱으로 중복 저장 방지.
*   **표준 준수**: `pyproject.toml` 표준을 따르며 Lock 파일 안정성 보장.

Happy coding with **uv**! 🚀
