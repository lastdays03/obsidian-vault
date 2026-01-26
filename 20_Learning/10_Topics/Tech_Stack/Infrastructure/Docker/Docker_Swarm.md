---
created: 2026-01-26
tags:
  - knowledge/topic
  - tech_stack/infrastructure
  - devops
  - tool/docker-swarm
  - orchestration
Source: User Prompt
---

# Docker Swarm

## Definition
**Docker Swarm**은 Docker Engine에 내장된 **네이티브 클러스터링 및 오케스트레이션 도구**입니다. 여러 대의 서버(Node)를 하나의 가상 호스트처럼 묶어 컨테이너 서비스를 분산 배포하고 관리합니다. Kubernetes에 비해 설정이 압도적으로 간편하여, **"Simple Production"**이 필요한 소규모 팀이나 온프레미스 환경에서 2026년에도 여전히 강력한 선택지입니다.

## Key Features
- **Built-in**: 별도 설치 없이 `docker swarm init` 명령어로 즉시 활성화됩니다.
- **Manager/Worker**: 매니저 노드(Raft 합의, 스케줄링)와 워커 노드(컨테이너 실행) 구조로 고가용성(HA)을 보장합니다.
- **Service**: `docker service create`를 통해 선언적으로 배포하며, 자동 복구(Rescheduling)와 로드밸런싱을 제공합니다.
- **Routing Mesh**: 어떤 노드로 요청이 들어오더라도 실행 중인 컨테이너로 트래픽을 자동 라우팅합니다.
- **Secrets**: 민감한 정보(암호, 키)를 암호화하여 안전하게 컨테이너에 전달합니다.

## Docker Swarm vs Kubernetes (2026)
| Feature        | Docker Swarm         | Kubernetes                  |
| -------------- | -------------------- | --------------------------- |
| **Complexity** | Low (Easy to start)  | High (Steep learning curve) |
| **Setup**      | `docker swarm init`  | Kubeadm, K3s, Helm, etc.    |
| **Use Case**   | Small/Medium Cluster | Enterprise / Cloud Native   |

## Basic Usage
```bash
# Initialize Manager
docker swarm init --advertise-addr <IP>

# Join Worker (on worker node)
docker swarm join --token <TOKEN> <IP>:2377

# Deploy Service
docker service create --replicas 3 --name web -p 80:80 nginx

# Stack Deploy (from Compose file)
docker stack deploy -c compose.yaml myapp
```

## Links
- [[Docker_MOC]]
- [[Docker]]
- [[Docker_Compose]] (Compatible format)
