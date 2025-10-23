# 🌐 2025-demo-01 — Digital Asset Exchange Infra (2025-09-26~ing)

> “24/365 멈추지 않는 거래소 인프라를 코드로 운영한다.”
> 
> 
> Kubernetes × Terraform × Argo CD 기반, 
> 
> **실제 거래소 수준은 아니지만 도전적인 SophieLABs.**
> 

---

## 핵심 목표!

- Multi-env (dev/stg/prod) GitOps 완전 자동화
- sync-wave 기반 **배포 순서 시각화 (mesh→policy→services→DR→observability)**
- Secret은 전부 **SOPS + AGE + IRSA SSM 연동**
- CI 단계별 **kubeconform / yamllint / trivy scan 자동 검증**
- DR 리허설 / Route53 failover 시뮬레이션 코드화
- Kyverno + OPA 기반 운영 정책 준수

## System Overview

<img width="1024" height="1024" alt="Image" src="https://github.com/user-attachments/assets/0bae4e68-d010-4308-bd45-3aa44d5ba2cd" />

---

## Repository (In Progress!! )

| Category | Repository | Description |
| --- | --- | --- |
| Control Tower | [platform-argocd](https://github.com/2025-demo-01/platform-argocd?utm_source=chatgpt.com) | Argo CD App-of-Apps, sync-wave 규칙 정립 (10→90). |
|  Infra IaC | [infra-terraform](https://github.com/2025-demo-01/infra-terraform?utm_source=chatgpt.com) | AWS EKS/MSK/Aurora/ClickHouse 모듈화 + Multi-stage env 구성. |
|  API Gateway | [svc-gateway](https://github.com/2025-demo-01/svc-gateway) | Istio + Envoy + JWT 인증, RateLimit + Canary 배포. |
|  Trading API | [svc-trading-api](https://github.com/2025-demo-01/svc-trading-api) | 주문 처리, Kafka publish, trace-id 헤더 전파. |
| Matching Engine | [svc-matching-engine](https://github.com/2025-demo-01/svc-matching-engine) | Rust 기반 저지연 엔진, NUMA-aware, Kafka/ClickHouse 연동. |
| Wallet Service | [svc-wallet](https://github.com/2025-demo-01/svc-wallet) | FastAPI + Kafka 소비, MPC mock, Aurora 잔액관리. |
| Risk Control | [svc-risk-control](https://github.com/2025-demo-01/svc-risk-control) | 실시간 한도/리스크 시뮬레이션 (Kafka Stream + Alert hook). |
| Observability | [observability-stack](https://github.com/2025-demo-01/observability-stack) | Prometheus/Loki/Tempo + eBPF 이상탐지 + Grafana 대시보드. |
|  Policy as Code | [policy-as-code](https://github.com/2025-demo-01/policy-as-code) | Kyverno/OPA + 이미지 서명 + 네임스페이스 격리. |
| DR / Chaos Test | [tests-and-dr](https://github.com/2025-demo-01/tests-and-dr) | RPO/RTO probe + DLQ reprocess + Failover rehearsal. |
| Data Pipeline | [data-pipeline](https://github.com/2025-demo-01/data-pipeline) | Debezium → Kafka → Flink/Faust → ClickHouse ETL. |
| Architecture Docs | [exchange-architecture](https://github.com/2025-demo-01/exchange-architecture) | 전체 시스템 다이어그램, SLO/SLA 문서화. |

---

## 앞으로 계속 해야하는거 ! (계속 적어나가야함)

- [ ]  svc-risk-control → Kafka Stream 기반 실시간 리스크 시뮬레이터
- [ ]  observability-stack → wallet/matching 지표 대시보드 통합
- [ ]  exchange-architecture → 시각적 다이어그램 + 시스템 문서화
- [ ]  FinOps (Opencost) 및 SLO Error Budget tracking 추가
- [ ]  Argo Rollouts 기반 Blue/Green 배포 실험

---

마지막 업데이트일 : 2025-10-23 16:35 KST
