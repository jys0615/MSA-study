# Dandelion v2
> MSA 학습에서 시작해 실제 서비스로 진화하는 레포지토리입니다.

---

## 이 레포지토리에 대하여

이 레포지토리는 두 단계로 진행됩니다.

**초기 릴리즈** — MSA 핵심 개념을 직접 구현하며 체득합니다.
**후반 릴리즈** — 학습한 내용을 바탕으로 Dandelion v2 서비스를 구축합니다.

공부와 프로젝트를 분리하지 않습니다. 각 개념을 익히는 과정 자체가 v2의 기반이 됩니다.

---

## 시작하게 된 이유

2025년 K-HTML 해커톤에서 **[Dandelion](https://github.com/jys0615/Dandelion)** 이라는 프로젝트를 만들었습니다.

동대문구 이문 뉴타운 주민 민원을 AI가 자동으로 분류하고 청원서를 생성해주는 서비스였는데, 짧은 시간 안에 기능을 구현하다 보니 자연스럽게 서버가 셋으로 쪼개졌습니다.

```
Spring Boot  →  인증, DB, REST API 오케스트레이션
FastAPI      →  LangChain 기반 LLM 추론, SSE 스트리밍
Node.js      →  북마클릿 자동화, 외부 청원 플랫폼 연동
```

세 서버는 Docker Compose로 묶었고, 상태 플래그 기반 이벤트 구조로 DB 일관성을 유지했습니다.

동작은 했습니다. 그런데 개발하면서 계속 찜찜한 부분이 남았습니다.

> *서비스가 서로를 어떻게 찾아야 할까?*
> *FastAPI 서버가 죽으면 Spring이 어떻게 대응해야 할까?*
> *배포할 때 세 서버를 어떻게 독립적으로 올려야 할까?*

그때 눌러둔 질문들에 제대로 답하면서, 동시에 v2를 완성하기 위해 이 레포를 시작했습니다.

---

## 릴리즈 로드맵

### 📦 v0.x — MSA 기반 학습 (현재)

각 주제마다 **문제 정의 → 나이브한 구현 → 한계 확인 → 패턴 적용 → 비교** 순서로 진행합니다.

| 릴리즈 | 주제 | 상태 |
|--------|------|------|
| v0.1 | 서비스 분리와 통신 (REST, gRPC, Eureka, Gateway) | 🔲 예정 |
| v0.2 | 장애 격리와 복원력 (Circuit Breaker, Retry, Fallback) | 🔲 예정 |
| v0.3 | 비동기 이벤트 (Kafka, Event-Driven Architecture) | 🔲 예정 |
| v0.4 | 분산 데이터 전략 (Database per Service, CQRS, Redis) | 🔲 예정 |
| v0.5 | 배포와 운영 (K8s, CI/CD, Zipkin, Grafana) | 🔲 예정 |

### 🌼 v1.0 — Dandelion v2 서비스 출시

v0.x에서 쌓은 기반 위에 실제 서비스를 구축합니다.

---

## 학습 방법론

단순히 개념을 정리하는 것이 아니라, **직접 구현하면서 왜 이 패턴이 필요한지** 체득합니다.

각 주제마다 다음 순서를 따릅니다.

```
문제 상황 정의 → 나이브한 구현 → 한계 확인 → 패턴 적용 → 비교
```

---

## 기술 스택

- **Language**: Java 21
- **Framework**: Spring Boot 3.x, Spring Cloud
- **AI**: Spring AI, Claude API
- **Message Broker**: Apache Kafka
- **Cache**: Redis
- **DB**: PostgreSQL, pgvector
- **Container**: Docker, Kubernetes
- **Monitoring**: Prometheus, Grafana, Zipkin

---

## 디렉토리 구조 (진행하면서 추가할 예정입니다)

```
dandelion-v2/
├── study/
│   ├── 01-service-discovery/
│   ├── 02-api-gateway/
│   ├── 03-circuit-breaker/
│   ├── 04-kafka-event/
│   ├── 05-saga-pattern/
│   ├── 06-cqrs/
│   └── 07-k8s-deploy/
└── services/               ← v1.0부터 추가
    ├── api-gateway/
    ├── auth-service/
    └── ...
```

---

## 연결 프로젝트

| 프로젝트 | 설명 |
|---------|------|
| [Dandelion](https://github.com/jys0615/Dandelion) | 이 레포의 출발점이 된 해커톤 프로젝트 |

---

---

## 문서

| 문서 | 설명 |
|------|------|
| [Troubleshooting](docs/TROUBLESHOOTING.md) | 개발 중 마주한 문제와 해결 과정 |

---

*학습한 내용은 각 디렉토리 README와 커밋 메시지에 기록합니다.*
