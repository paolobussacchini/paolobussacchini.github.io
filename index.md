---
layout: page
title: Home
permalink: /
---

# Paolo Bussacchini

**Platform / Infrastructure Software Engineer** · Brescia, Italy

[paolobussacchini@gmail.com](mailto:paolobussacchini@gmail.com)
| [LinkedIn](https://www.linkedin.com/in/paolo-bussacchini)
| [GitHub](https://github.com/paolobussacchini)

Platform engineer with 4+ years building and operating cloud-native systems. Production experience with self-managed k3s and AWS EKS, Helm packaging, GitOps (Argo CD), and GitHub Actions. Contributed to a multi-tenant enterprise PaaS with strong on-prem deployment needs, and to financial-grade services at a capital markets firm. Focused on reliable delivery, observability, and developer experience.
Deepening Go and infrastructure-as-code.

---

## Experience

### Software Developer (Platform) · SmeUP
**Mar 2024 - Present** · Erbusco, Italy

- Operate production Kubernetes for a multi-tenant PaaS: self-managed k3s (on-prem / customer-shaped) and AWS EKS - packaging, rollouts, and day-2 operations across environments.
- Co-authored the platform's official Helm chart to standardize enterprise installs (defaults, config boundaries, lifecycle) and reduce deployment drift.
- Ship via Argo CD GitOps and GitHub Actions, keeping cloud and on-prem environments consistent and reviewable from Git.
- Designed platform auth with OIDC / Keycloak (Google Workspace); balanced on-prem customer reality vs. speculative multi-tenant complexity.
- Integrated SFTPGo into Helm charts (StatefulSet, storage, networking); debug production connectivity across app, container, Kubernetes, and firewall layers.
- Built platform services (auth, routing, batch)

*Java · TypeScript · Docker · Kubernetes · k3s · EKS · Helm · Argo CD · GitHub Actions · AWS ECR · Keycloak/OIDC · OpenTelemetry · Jaeger · PostgreSQL · MongoDB*

### Java Developer · Perigea @ GateLab / Banca IMI
**Apr 2022 - Mar 2024** · Milan, Italy

- Spring Boot microservices for trading/investment-desk apps at Banca IMI (Euronext), under strict latency, reliability, and change-control expectations.
- Real-time bond/derivatives metrics (Java + PL/SQL) and Java-C++ bridges via Protocol Buffers; strong test coverage and reviews; Jenkins CI.

*Java · Spring Boot · Protocol Buffers · Jenkins · Oracle DB · React*

---

## Projects

### sre-lab - Terraform · Kubernetes · Go · Argo CD *(in progress)*

Personal platform lab: IaC foundations, Go service with health/metrics, GitOps sync onto lightweight Kubernetes - practicing the laptop->cluster delivery loop.

First write-up: [Starting an SRE lab](/2026/10/18/starting-an-sre-lab/) · [All posts](/blog/)

---

## Blog

Notes from labs and learning.

{% for post in site.posts limit:3 %}
- **[{{ post.title }}]({{ post.url · relative_url }})** - {{ post.date · date: "%b %-d, %Y" }}
{% endfor %}

[Browse all posts ->](/blog/)

---

## Technical skills

| Area | Skills |
|---|---|
| Platform / K8s | Kubernetes · k3s · EKS · Helm · Argo CD / GitOps · Docker · CRDs · Ingress · RBAC |
| CI/CD & Cloud | GitHub Actions · Jenkins · AWS (EKS, ECR) · Terraform *(learning via labs)* |
| Observability | OpenTelemetry · Jaeger · metrics/logging mindset · Prometheus *(learning)* |
| Languages | Java *(proficient)* · TypeScript · SQL · Go *(actively learning)* · Bash |
| Security / IdP | OIDC · Keycloak · secrets/config hygiene · enterprise on-prem constraints |
| Backend / Data | Spring Boot · REST · gRPC · Protobuf · RabbitMQ · PostgreSQL · MongoDB · Oracle |

---

## Education

**B.Sc. Statistics (110/110 cum laude)** · University of Milano-Bicocca  
Sep 2017 - Sep 2020 · Milan  

Probability, Computer Science, Statistical Models, Databases.
