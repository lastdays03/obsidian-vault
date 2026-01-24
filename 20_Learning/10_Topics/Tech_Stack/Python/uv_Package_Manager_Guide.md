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

## 0. Clean Migration (Critical)

기존의 `pyenv`, `conda` 등이 섞여 있는 환경을 완전히 정리하고 `uv`로 깨끗하게 전환하는 방법입니다. (2026년 1월 기준 최신 방식)

### 1단계. 기존 도구 완전 제거 (순서 중요!)

**1. 쉘 설정에서 pyenv / conda 초기화 코드 제거**

```bash
# ~/.bashrc, ~/.zshrc, ~/.bash_profile, ~/.zprofile 등 열기
nano ~/.zshrc    # 또는 vim / code 등

# 아래와 비슷한 줄 전부 주석 처리하거나 삭제
# export PATH="$HOME/.pyenv/bin:$PATH"
# eval "$(pyenv init -)"
# eval "$(pyenv virtualenv-init -)"
# export PATH="$HOME/anaconda3/bin:$PATH"   # 또는 miniconda3
# . "$HOME/anaconda3/etc/profile.d/conda.sh"
# conda deactivate  # 등
```

→ 파일 저장 후 터미널 재시작 또는 `source ~/.zshrc` 실행.

**2. pyenv 완전 삭제**

```bash
# pyenv 자체 제거 (설치된 python들도 같이 날아감)
rm -rf ~/.pyenv

# pyenv-virtualenv가 따로 있었다면
rm -rf ~/.pyenv/plugins/pyenv-virtualenv
```

**3. Anaconda / Miniconda 완전 삭제**

```bash
# 가장 안전한 방법 (conda 자체가 제공하는 clean 방법)
# conda가 아직 살아있다면 먼저 실행
conda install anaconda-clean   # (안 되면 생략)
anaconda-clean --yes

# 본체 삭제 (설치 경로에 따라 선택)
rm -rf ~/anaconda3
rm -rf ~/miniconda3
rm -rf ~/opt/anaconda3         # 일부 Mac 설치 경로
rm -rf ~/opt/miniconda3

# conda가 남긴 캐시 / 백업 등도 삭제 (선택)
rm -rf ~/.conda
rm -rf ~/.condarc
rm -rf ~/.continuum
```

**4. pip 캐시 / uv 이전 캐시 정리 (선택적이지만 추천)**

```bash
rm -rf ~/.cache/pip
rm -rf ~/.cache/uv
rm -rf ~/.local/share/uv
```

**5. 쉘 재시작 & PATH 확인**

```bash
exec $SHELL -l          # 또는 새 터미널 열기

which python            # /usr/bin/python 또는 시스템 python 나와야 함
which pip               # /usr/bin/pip 또는 없어야 좋음
python --version
conda --version         # command not found 나와야 함
pyenv --version         # command not found 나와야 함
```

여기까지 하면 이전 환경이 거의 깨끗이 지워진 상태입니다.

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
