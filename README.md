# DevOps + AI-in-DevOps Mastery Roadmap (2026 Edition)

> **Goal = **Learn DevOps**
---

## The 2026 tech stack we must cover

| Layer | Tools you'll actually learn |
|---|---|
| OS & scripting | Linux (Ubuntu), Bash, **Python** (+ boto3) |
| Networking | TCP/IP, DNS, HTTP/HTTPS/TLS, load balancers, reverse proxy, API gateway |
| Version control | Git, GitHub (+ GitHub Flow, PRs) |
| Cloud | **AWS** (IAM, EC2, VPC, S3, RDS, Lambda, EKS, Bedrock) + **LocalStack** for free local practice |
| IaC | **Terraform / OpenTofu**, Ansible |
| Containers | **Docker**, Docker Compose |
| Orchestration | **Kubernetes** (local via **kind/minikube**, cloud via **EKS**), Helm |
| CI/CD | **GitHub Actions** (primary), Jenkins, GitLab CI |
| GitOps | **Argo CD**, Flux |
| Observability | **Prometheus, Grafana, Loki, OpenTelemetry**, CloudWatch |
| Reliability (SRE) | SLI/SLO/error budgets, alerting, incident response |
| Security (DevSecOps) | Trivy, **HashiCorp Vault**, OPA/**Kyverno**, SAST/DAST/SCA, supply-chain (SBOM, Sigstore) |
| **AI in DevOps** | **AIOps** (Datadog Watchdog/Bits AI, Dynatrace Davis AI, PagerDuty AIOps), **AI observability** (drift, LLM evals), **agentic ops** (CrewAI/LangGraph, **MCP**, RAG-for-runbooks), **LLMOps** (Bedrock, model serving, vector DBs) |
| Platform Eng | Internal Developer Platforms, **Backstage**, golden paths |

---

## The roadmap

### PHASE 1 — Core Foundations
*Goal: think like an engineer, automate confidently, understand how systems talk.*

- **01 · DevOps Fundamentals & the 2026 Landscape** — what DevOps really is (culture + practice), the SDLC, DevOps vs SRE vs Platform Engineering, and exactly where AI now fits. *(~2–3 days)*
- **02 · Linux Essentials for DevOps** — filesystem, permissions, processes, `systemd`, package management, logs, text-processing (`grep`/`sed`/`awk`). *(~1 week)*
- **03 · Bash Shell Scripting** — variables, conditionals, loops, functions, exit codes; write real automation scripts. *(~4–5 days)*
- **04 · Python for DevOps** — scripting, files, APIs/JSON, `requests`, and `boto3` to drive AWS. *(~1 week)*
- **05 · Networking Fundamentals** — OSI & TCP/IP, IP/subnets/CIDR, DNS, HTTP/HTTPS, TLS, ports, load balancers, reverse proxy, API gateway, firewalls; `dig`/`curl`/`traceroute`. *(~1 week)*
- **06 · Git & GitHub** — version control deeply, branching, merges/rebases, PRs, collaboration workflows. *(~3–4 days)*

### PHASE 2 — Cloud & Infrastructure
*Goal: provision and run real infrastructure, declaratively.*

- **07 · AWS Fundamentals** — account & billing safety, **IAM**, **EC2**, **VPC/networking**, **S3**, **RDS**; practice free with **LocalStack** + AWS Free Tier. *(~2 weeks)*
- **08 · Infrastructure as Code: Terraform / OpenTofu** — providers, state, variables, modules; provision real AWS infra. *(~1.5 weeks)*
- **09 · Configuration Management: Ansible** — inventories, playbooks, roles, idempotency. *(~1 week)*

### PHASE 3 — Containers & Orchestration
*Goal: package apps and run them at scale.*

- **10 · Docker & Containerization** — images, Dockerfiles, layers, registries, networking, volumes, Compose. *(~1.5 weeks)*
- **11 · Kubernetes Fundamentals** — pods, deployments, services, ingress, ConfigMaps/Secrets, namespaces; local via **kind/minikube**. *(~2 weeks)*
- **12 · Kubernetes in Production** — Helm, autoscaling (HPA/**KEDA**), storage, RBAC, operators; deploy on **AWS EKS**. *(~2 weeks)*

### PHASE 4 — Delivery (CI/CD & GitOps)
*Goal: ship safely and automatically.*

- **13 · CI/CD with GitHub Actions** — pipelines, build/test/deploy, secrets, matrices, environments. *(~1.5 weeks)*
- **14 · Jenkins & GitLab CI** — the enterprise tools you'll be interviewed on. *(~1 week)*
- **15 · GitOps with Argo CD / Flux** — Git as source of truth, declarative deploys, drift detection, instant rollback. *(~1 week)*

### PHASE 5 — Observability & Reliability
*Goal: see everything; keep it up.*

- **16 · Monitoring & Observability** — the 3 pillars (metrics/logs/traces), **Prometheus, Grafana, Loki, OpenTelemetry**, dashboards, alerting; CloudWatch. *(~2 weeks)*
- **17 · SRE Practices** — SLI/SLO/error budgets, alert design, incident response, on-call, blameless postmortems. *(~1 week)*

### PHASE 6 — Security (DevSecOps)
- **18 · DevSecOps** — shift-left security, SAST/DAST/SCA, image scanning (**Trivy**), secrets (**Vault**), policy-as-code (**Kyverno/OPA**), supply-chain security (SBOM, signing). *(~1.5 weeks)*

### PHASE 7 — AI in DevOps (the 2026 differentiator) 🤖
*Goal: do the thing most candidates can't yet — operate AI, and operate WITH AI.*

- **19 · AI-Assisted DevOps & AIOps Fundamentals** — what AIOps is, anomaly detection, alert correlation & noise reduction, root-cause analysis, the **trust gradient** (read-only → scoped automation → autonomy); tools: Datadog Watchdog/**Bits AI**, Dynatrace **Davis AI**, PagerDuty AIOps, New Relic. *(~1 week)*
- **20 · AI Observability — Monitoring AI/ML Systems** — why "green dashboards" lie for ML, **model & data drift**, hallucination/quality monitoring, **LLM observability & evals**, cost/latency of inference. *(~1 week)*
- **21 · Building & Deploying AI Agents for Ops** — agentic DevOps, frameworks (**CrewAI / LangGraph**), **MCP servers** for tool access, **RAG-for-runbooks**, self-healing patterns, guardrails & human-in-the-loop. *(~2 weeks)*
- **22 · Deploying AI/ML & GenAI Workloads (MLOps/LLMOps)** — model packaging & serving, **AWS Bedrock + Lambda**, GPU/inference basics, **vector databases**, CI/CD for models, scaling & cost control. *(~2 weeks)*

### PHASE 8 — Platform Engineering & Career Launch
- **23 · Platform Engineering & Developer Experience** — Internal Developer Platforms, **Backstage**, golden paths, self-service. *(~1 week)*
- **24 · Capstone, Portfolio & Interview Prep** — one end-to-end project that uses *everything*, plus resume, GitHub portfolio polish, interview Q&A, and which certs are worth it. *(~2 weeks)*

---

## Portfolio projects (mapped to the job goal)

Employers hire on **proof**. We'll build these as we go, so by the end you have a GitHub portfolio that tells a story:

1. **Foundations:** a Bash/Python automation toolkit + a documented home lab.
2. **Cloud + IaC:** a complete AWS environment provisioned with Terraform (VPC, EC2, S3, RDS).
3. **Containers + K8s:** a multi-service app on Kubernetes (local → EKS) with Helm.
4. **Full pipeline:** GitHub Actions CI/CD → Argo CD GitOps deploy to your cluster.
5. **Observability stack:** Prometheus + Grafana + OpenTelemetry with real SLOs and alerts.
6. **AI capstone:** an **AIOps incident-analysis assistant** — pulls logs/metrics, detects anomalies, and uses an LLM + RAG over your runbooks to suggest (and optionally execute, with approval) a fix. *This is the project that makes a 2026 resume stand out.*

---

## Timeline & expectations

- At a **steady part-time pace** (1–2 focused hours/day), expect roughly **6–9 months** to job-ready — faster if you go full-time. You have a head start with Linux/coding.
- There's **no rush** — depth beats speed. We finish each module properly before moving on.

## Certifications worth considering (optional, after the relevant phases)

- **AWS Certified Cloud Practitioner** → then **AWS SysOps / DevOps Engineer – Professional** (after Phases 2–4).
- **Certified Kubernetes Administrator (CKA)** (after Phase 3) — strong signal for K8s roles.
- **HashiCorp Terraform Associate** (after Module 08).
- *Note: experience + a real portfolio beat certs alone — but a cert or two helps you pass resume screens.*

---

## Progress tracker

```
PHASE 1 — Foundations
[ ] 01  DevOps Fundamentals & 2026 Landscape
[ ] 02  Linux Essentials
[ ] 03  Bash Scripting
[ ] 04  Python for DevOps
[ ] 05  Networking Fundamentals
[ ] 06  Git & GitHub

PHASE 2 — Cloud & Infrastructure
[ ] 07  AWS Fundamentals
[ ] 08  Terraform / OpenTofu
[ ] 09  Ansible

PHASE 3 — Containers & Orchestration
[ ] 10  Docker
[ ] 11  Kubernetes Fundamentals
[ ] 12  Kubernetes in Production (EKS)

PHASE 4 — Delivery
[ ] 13  CI/CD with GitHub Actions
[ ] 14  Jenkins & GitLab CI
[ ] 15  GitOps (Argo CD / Flux)

PHASE 5 — Observability & Reliability
[ ] 16  Monitoring & Observability
[ ] 17  SRE Practices

PHASE 6 — Security
[ ] 18  DevSecOps

PHASE 7 — AI in DevOps
[ ] 19  AIOps Fundamentals
[ ] 20  AI Observability
[ ] 21  AI Agents for Ops
[ ] 22  Deploying AI/ML & GenAI (MLOps/LLMOps)

PHASE 8 — Platform Eng & Career
[ ] 23  Platform Engineering
[ ] 24  Capstone, Portfolio & Interviews
```

---
