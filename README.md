# 2025-demo-01

> 디지털 자산 거래소 인프라를 실험적으로 구축하는 나만의 실험실... 
> 모든 서비스는 **쿠버네티스 기반**이며, **IaC(Terraform)** 로 완전히 자동화해야한다.  
## 한마디로 정리하자면...
> “ 24/365 멈추지 않는 그런 서비스 인프라를 코드로 관리하겠어.”  
> 장애, 보안, 확장성, 복구, 모든 걸 자동화해버리는 나만의....실험실..
> (바이낸스가 놀랄지도 몰라...) 왜냐고? GPT가 바이낸스구조로 도와준다 했다고. 

---

## SophieLab 목표

- **목표** 24/365 무중단 서비스 인프라  
- **핵심 가치** 코드로 관리되는 신뢰성, 보안성, 복구 가능성  
- **배포 전략** GitOps + ArgoCD + Flagger + Istio Canary  
- **운영 환경** dev / stg / prod (완전 격리형 환경)

---

## Repository 12개가 넘을 거다.

### infra-terraform
인프라 전반을 Terraform으로 관리한다.
- EKS, MSK, Aurora, ClickHouse, Opencost, DR 전부 코드화  
- `envs/{dev,stg,prod}` 구조로 multistage 배포  
- Karpenter, IRSA, Route53 Failover 등 포함  
- modules 기반으로 유지보수성 강화

---

### platform-argocd
GitOps Control Tower.
- ArgoCD App-of-Apps 구조  
- 서비스 별(Trading, Wallet, Risk, Matching) 배포 정의  
- targetRevision 고정으로 환경별 안정성 확보  
- 운영자 승인 후만 sync 가능 (manual sync policy)

---

### observability-stack
Monitoring / Logging 전체 Stack.
- Prometheus, Loki, Tempo, Grafana  
- SLO/SLI 기반 알람 + Error budget burn 계산  
- eBPF 기반 이상탐지용 규칙 포함  
- Grafana Dashboard 일괄 관리 (JSON)

---

### policy-as-code
보안 정책 통합 관리.
- Kyverno + Gatekeeper + OPA 규칙  
- Trivy 이미지 스캔, SBOM 검증, cosign 서명 필수  
- SOPS로 Secret 암호화  
- 공급망 보안(Supply Chain Security) 자동 검증 pipeline 포함

---

### svc-matching-engine
거래 매칭 엔진.
- Rust 기반 저지연 처리  
- CPU pinning / NUMA-aware Pod 설정  
- HPA + PriorityClass 적용으로 HotPass 보장  
- p95 latency 목표: ≤ 50ms

---

### svc-trading-api
거래 API Gateway.
- Go 기반, Istio mTLS + JWT 인증  
- Flagger canary Deploy  
- SLO 측정 지표: API latency, error rate  
- ExternalDNS + ALB Controller 연동

---

### svc-wallet
입출금 지갑 서비스.
- Python(FastAPI) 기반  
- Kafka/Redis 기반 트랜잭션 큐  
- SOPS 암호화된 Secret (MPC/HSM mock 포함)  
- Withdraw Queue Delay SLA: p95 ≤ 120s

---

### svc-risk-control
위험 관리 서비스.
- Kafka Stream 소비 + 실시간 Risk 계산  
- Flink/Faust 기반 파이프라인 연동  
- Alert → Slack/Webhook 자동 통보  
- 정책 위반 시 거래 차단 Simulation 포함

---

### svc-gateway
내부 서비스 진입점(API Gateway).
- Istio Gateway + Envoy RateLimit Service  
- Policy Enforcement Sidecar  
- 인증, 로깅, 라우팅 전담  
- Geo Routing + Cloudflare WAF 연계 가능

---

### data-pipeline
데이터 파이프라인 관리.
- Debezium → Kafka → Flink/Faust → ClickHouse  
- CDC, 스트리밍 분석, 거래 Log 적재  
- ETL Job 자동화 (Argo Workflow 기반)

---

### tests-and-dr
장애 대응 및 DR 자동화.
- RPO/RTO 측정 및 검증  
- Route53 Failover 테스트  
- Chaos Mesh 기반 장애 주입 시나리오  
- DR 리허설 주기적 실행(CronJob)

---

## 기술 스택 요약

| 말만번지르 |  기술 |
|------|------------|
| **Infra as Code** | Terraform, AWS(EKS, MSK, Aurora, KMS, Route53) |
| **GitOps** | ArgoCD, Helm, Kustomize |
| **Observability** | Prometheus, Grafana, Loki, Tempo, Alertmanager |
| **Security** | Kyverno, OPA, Trivy, cosign, SOPS, Vault |
| **Scalability** | Karpenter, HPA, Spot/OnDemand 자동 조합 |
| **Resilience** | Route53 Failover, Aurora Global DB, DR Automation |
| **Delivery** | Flagger, Istio, Blue-Green/Canary |
| **Data Pipeline** | Kafka, Flink/Faust, ClickHouse |
| **Wallet Security** | MPC/HSM mock, SOPS Secret, Redis Queue |
| **Runtime Protection** | Falco, eBPF-based anomaly detection |

---

## 현재 진행 중

- [x] Terraform 기반 prod 환경 완성 (`infra-terraform/envs/prod`)
- [x] ArgoCD App-of-Apps 구조 구성
- [x] Observability Stack (v0.9.0) 적용
- [ ] Flagger + Istio Canary 정책 세부 튜닝 중
- [ ] Wallet 서비스 HSM mock 강화 예정
- [ ] Multi-Region DR rehearsal 주간화 목표

---


