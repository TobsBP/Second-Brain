---
type: job-concept
job: MulherEmForma
tags: [strapi, core-domain]
updated: 2026-07-21
---

# mef-core-api-v1

Strapi — o **core de regras de negócio** da MEF: avaliações (XRay), geração de treinos e dietas, controle de assinaturas e pagamentos.

## Arquitetura
`REST API → Controllers → Services → Entities`, organizado em módulos de domínio: XRay, Training Generator, Diet Generator, Subscriptions. Postgres como banco. Consumido pelo [[wiki/job/MulherEmForma/bff]] e por um gateway de pagamento externo (Pay).

## Sources
- [[raw/job/MulherEmForma/mef-core-api-v1.md]]
