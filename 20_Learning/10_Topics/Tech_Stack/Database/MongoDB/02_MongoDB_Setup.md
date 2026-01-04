---
tags: [knowledge/topic, database/mongodb, environment/docker]
Up: [[MongoDB_MOC]]
Source: https://github.com/lastdays03/study_mongodb/blob/main/study/02_setup.md
---

# 02. MongoDB Setup (환경 구축)

## 📖 개요 (Overview)
MongoDB 학습 환경을 구축하는 방법입니다. 로컬 설치보다 관리가 쉬운 **Docker** 방식을 권장합니다.

## 💡 설치 방법 (Docker)
Docker가 설치된 환경에서 아래 명령어로 즉시 실행할 수 있습니다.

```bash
# MongoDB 컨테이너 실행 (포트 27017)
docker run -d -p 27017:27017 --name my-mongo mongo:latest
```

## 🔧 기타 도구
- **MongoDB Compass**: 데이터를 시각적으로 관리할 수 있는 공식 GUI 도구입니다.
    - 접속 URI: `mongodb://localhost:27017`
- **MongoDB Atlas**: 설치 없이 사용할 수 있는 클라우드 관리형 서비스입니다.

## 💡 Key Insights
- **Containerization**: 데이터베이스를 도커 컨테이너로 띄우면, 개발 환경을 더럽히지 않고 여러 버전을 쉽게 테스트할 수 있습니다.
