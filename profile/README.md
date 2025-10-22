# 2025-demo-01

> 디지털 자산 거래소 인프라를 실험적으로 구축하는 나만의 실험실... 
> 모든 서비스는 **Kubernetes 기반**이며, **IaC(Terraform)** 로 완전히 자동화해야한다.  
## 한마디로 정리하자면...
> “ 24/365 멈추지 않는 그런 서비스 인프라를 코드로 관리하겠어.”  
> 장애, 보안, 확장성, 복구, 모든 걸 자동화해버리는 나만의....실험실..
> (바이낸스가 놀랄지도 몰라...) 왜냐고? GPT가 바이낸스구조로 도와준다 했다고. 

---
```markdown
[Client] 
  │ HTTP(S)
  ▼
[svc-gateway  (Istio Ingress + JWT + RateLimit)]
  │  routes (Istio VS)
  ├──────────────▶ [svc-trading-api  (REST/gRPC)]
  │                    ├─ produce ▶ Kafka (MSK): orders.in
  │                    └─ read/write ▶ Aurora (orders, accounts)
  │
  └──────────────▶ [svc-wallet      (FastAPI)]
                       ├─ consume ◀ Kafka: withdraw.req
                       ├─ HSM/MPC (mock) 서명
                       └─ read/write ▶ Aurora (balances)
                                │
                                ▼
                     [svc-matching-engine (Rust)]
                       ├─ consume ◀ Kafka: orders.in
                       ├─ produce ▶ Kafka: trades.out, book.events
                       └─ ClickHouse insert (ticks/logs)
                                │
                                ▼
                     [svc-risk-control]
                       ├─ consume ◀ Kafka: orders.in, trades.out
                       ├─ fetch ▶ Redis/Feature Store
                       └─ decision ▶ Kafka: risk.decisions
                                
Infra (Terraform):
  EKS + MSK + Aurora + ClickHouse + SSM + IAM(IRSA) + ALB + Route53(+ARC)
GitOps (platform-argocd):
  App-of-Apps로 모든 서비스/정책/관찰성 배포
Observability:
  Prometheus(/metrics), Loki(stdout), Tempo(Trace), Grafana 대시보드
Policy-as-Code:
  Kyverno/OPA → 이미지서명/PSP/네트워크/Secrets 강제
Tests-and-DR:
  ChaosMesh, ARC 토글 리허설, Route53 Failover

```

