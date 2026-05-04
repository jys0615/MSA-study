# MSA Study
> Microservice Architecture 학습 레포지토리

---

## 시작하게 된 이유

2025년 K-HTML 해커톤에서 **[Dandelion](https://github.com/jys0615/Dandelion)** 이라는 프로젝트를 만들었다.

동대문구 이문 뉴타운 주민 민원을 AI가 자동으로 분류하고 청원서를 생성해주는 서비스였는데, 짧은 시간 안에 기능을 구현하다 보니 자연스럽게 서버가 셋으로 쪼개졌다.

```
Spring Boot  →  인증, DB, REST API 오케스트레이션
FastAPI      →  LangChain 기반 LLM 추론, SSE 스트리밍
Node.js      →  북마클릿 자동화, 외부 청원 플랫폼 연동
```

세 서버는 Docker Compose로 묶었고, 상태 플래그 기반 이벤트 구조로 DB 일관성을 유지했다.

동작은 했다. 그런데 개발하면서 계속 찜찜한 게 남았다.

> *서비스가 서로를 어떻게 찾아야 하지?*
> *FastAPI 서버가 죽으면 Spring이 어떻게 대응해야 하지?*
> *배포할 때 세 서버를 어떻게 독립적으로 올려야 하지?*

그때 눌러둔 질문들에 제대로 답하기 위해 이 레포를 시작했다.

---

## 학습 목표

단순히 개념을 정리하는 것이 아니라, **직접 구현하면서 왜 이 패턴이 필요한지** 체득한다.

각 주제마다 다음 순서를 따른다.

```
문제 상황 정의 → 나이브한 구현 → 한계 확인 → 패턴 적용 → 비교
```

---

## 학습 로드맵

### 1단계 — 서비스 분리와 통신

- [ ] 모놀리식과 MSA의 경계 — 언제 쪼개야 하는가
- [ ] 동기 통신: REST vs gRPC 비교 구현
- [ ] 서비스 디스커버리: Eureka 등록/조회 직접 구현
- [ ] API Gateway: Spring Cloud Gateway 라우팅 / 필터

### 2단계 — 장애 격리와 복원력

- [ ] 서킷 브레이커: Resilience4j 적용 전후 비교
- [ ] Retry / Timeout / Fallback 패턴
- [ ] Bulkhead 패턴으로 장애 전파 차단

### 3단계 — 비동기 이벤트

- [ ] 왜 비동기가 필요한가 — Kafka 도입 이유 정리
- [ ] Kafka Producer / Consumer 기본 구현
- [ ] 이벤트 드리븐 아키텍처로 서비스 간 의존성 제거
- [ ] Saga 패턴으로 분산 트랜잭션 처리

### 4단계 — 데이터 전략

- [ ] 서비스별 DB 분리 (Database per Service)
- [ ] CQRS 패턴 구현
- [ ] 분산 캐싱: Redis 전략 비교

### 5단계 — 배포와 운영

- [ ] Docker Compose → Kubernetes 이식
- [ ] 서비스별 독립 CI/CD (GitHub Actions path filter)
- [ ] 분산 추적: Zipkin으로 서비스 간 호출 시각화
- [ ] Prometheus + Grafana 모니터링

---

## 기술 스택

- **Language**: Java 21
- **Framework**: Spring Boot 3.x, Spring Cloud
- **Message Broker**: Apache Kafka
- **Cache**: Redis
- **DB**: PostgreSQL
- **Container**: Docker, Kubernetes
- **Monitoring**: Prometheus, Grafana, Zipkin

---

## 디렉토리 구조 (진행하면서 추가 예정)

```
msa-study/
├── 01-service-discovery/
├── 02-api-gateway/
├── 03-circuit-breaker/
├── 04-kafka-event/
├── 05-saga-pattern/
├── 06-cqrs/
└── 07-k8s-deploy/
```

---

## 연결 프로젝트

| 프로젝트 | 설명 |
|---------|------|
| [Dandelion](https://github.com/jys0615/Dandelion) | MSA 공부를 시작하게 된 해커톤 프로젝트 |
| Dandelion v2 *(진행 예정)* | 이 레포의 학습을 적용한 풀스택 MSA 포트폴리오 |

---

*학습한 내용은 각 디렉토리 README와 커밋 메시지에 기록한다.*
