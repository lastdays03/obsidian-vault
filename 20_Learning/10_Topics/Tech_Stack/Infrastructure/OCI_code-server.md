# 개념 (Concept): OCI 기반 VS Code Server 구축

**태그**: #knowledge/concept #topic/Infrastructure #topic/Dev_Tools #topic/Cloud
**출처**: 사용자 제공 가이드 (Inbox)

---

## 📖 정의 (Definition)
**OCI 기반 VS Code Server 구축**은 오라클 클라우드(Oracle Cloud Infrastructure)의 평생 무료 A1 인스턴스(4코어, 24GB RAM) 인프라를 활용하여 언제 어디서나 브라우저만으로 접근 가능한 강력한 클라우드 코딩 환경을 구축하는 과정이다.

---

## 💡 예시 (Example)
핵심 구축 프로세스는 4단계로 요약된다.

### 1. 수동 설치 버전 삭제 (해당 시)
기존에 `systemd` 로컬 서비스 형태로 구동 중이라면 먼저 정리한다.
```bash
sudo systemctl stop code-server@$USER
sudo systemctl disable code-server@$USER
sudo apt remove code-server -y
sudo rm /usr/bin/code-server
rm -rf ~/.config/code-server
rm -rf ~/.local/share/code-server
```

### 2. OCI 방화벽 개방 (VCN 설정)
- OCI 콘솔 > 네트워킹 > VCN > 보안 목록(Security Lists) > Default Security List
- 수신 규칙 추가: 소스 `0.0.0.0/0`, TCP 포트 `8080` 허용.

### 3. Docker 엔진 및 권한 설정
```bash
# Docker 공식 스크립트로 설치
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

# 현재 접속 사용자에게 Docker 권한 부여
sudo usermod -aG docker $USER
newgrp docker
```

### 4. Docker Compose를 통한 VS Code Server 실행
관리를 위해 별도 디렉토리를 만들어 `docker-compose.yml`을 세팅한다.
```bash
mkdir -p ~/vscode-docker && cd ~/vscode-docker
nano docker-compose.yml
```

**`docker-compose.yml` 설정 예시**:
```yaml
version: "3.8"
services:
  code-server:
    image: lscr.io/linuxserver/code-server:latest
    container_name: code-server
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Seoul
      - PASSWORD=my-secure-password # 접속 비밀번호 변경 필수!
      - DEFAULT_WORKSPACE=/config/workspace
    volumes:
      - ./config:/config           # 확장 프로그램 및 내부 설정
      - ./project:/config/workspace # 실제 작업할 소스 코드 마운트
    ports:
      - 8080:8443
    restart: unless-stopped
```

저장 후 백그라운드로 실행:
```bash
docker compose up -d
```

### 5. SSH 터널링을 통한 안전한 로컬 접속 (Insecure Context 우회)
HTTP 환경(`http://IP:8080`)으로 바로 접속하면 브라우저 보안 정책에 의해 **클립보드 무단 복사/붙여넣기(`Insecure Context`) 기능이 차단**된다. 도메인+HTTPS가 없다면 **SSH 터널링**을 이용하는 것이 가장 확실하다.

**로컬 터미널(내 PC)에서 실행**:
```bash
ssh -L 8080:localhost:8080 -i [내_키_페어_경로] ubuntu@[OCI_공용_IP]
```

터널링 연결 유지 후, 브라우저에서 **`http://localhost:8080`**으로 접속하면 로컬 호스트 특권으로 모든 제약 없이 안전하게 코딩할 수 있다.

---

## ⚖️ 비교 (Comparison)
| Feature         | Local VS Code             | VS Code Server (OCI A1)                   | GitHub Codespaces                  |
| :-------------- | :------------------------ | :---------------------------------------- | :--------------------------------- |
| **접근성**      | 설치된 로컬 PC 전용       | 태블릿, 스마트폰 포함 브라우저 환경 제공  | 브라우저 및 로컬 VS Code 연결 가능 |
| **비용**        | 무료 (하드웨어 비용 제외) | **완전 무료** (OCI Always Free 티어 활용) | 무료 제공 시간 초과 시 과금        |
| **자원 사양**   | 로컬 시스템에 의존        | 4 Core, 24GB RAM (풍부한 사양 지속 제공)  | 유동적 (클라우드 파워 제공)        |
| **환경 제어권** | 완전한 권한 보유          | 루트 권한 포함, 전체 OS 인프라 완벽 제어  | 제한적인 컨테이너 구동 제어        |

## 🔑 Key Insights
- OCI ARM(Ampere) 인스턴스는 24GB의 엄청난 여유 램 덕분에, Docker 기반 컨테이너 환경으로 Code Server를 구동해도 시스템 자원의 1% 남짓(약 200~300MB)만 소비하여 남는 리소스를 데이터 분석, DB 구동 등에 마음껏 활용할 수 있다.
- Docker 볼륨 마운트(`~/vscode-docker` 폴더 매핑) 방식을 사용하면 서버를 포맷하거나 마이그레이션할 때 해당 폴더 하나만 백업하여 완벽한 복구가 가능하다.
- `linuxserver` 이미지는 오픈 릴리즈 마켓플레이스를 쓰므로 대부분의 확장이 있지만, 간혹 없는 확장 프로그램은 VSIX 파일을 직접 올려 수동 설치할 수 있다.
- 브라우저 기반으로 작동하지만 PWA 형태로 설치하면 데스크탑 네이티브 앱과 흡사한 UX/UI 경험을 누릴 수 있다.
- 기본 통신망이 HTTP이므로 클립보드 제약(Insecure Context)을 피하려면 **SSH 터널링**을 경유하거나 별도 도메인에 Nginx/Caddy를 엮어 HTTPS(SSL) 리버스 프록시를 씌워주어야 완벽한 생산성이 확보된다.

## 📚 References
```
