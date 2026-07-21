Levantado via GitHub API (`Mulher-em-Forma/mef-core-api-v1`) em 2026-07-21.

## README
"Aplicação Strapi responsável pelas regras de negócio centrais da plataforma. Gerencia dados e lógicas críticas, como o módulo de avaliações (XRay), geração de treinos e dietas, e o controle de assinaturas e pagamentos."

### Arquitetura (diagrama mermaid no README)
```
External: BFF, Pay (Payment Gateway)
Core API (Strapi):
  REST API
  Internal Layers: Controllers → Services → Entities
  Domain Modules: XRay, Training Generator, Diet Generator, Subscriptions
DB: PostgreSQL
```

## Relação
Consumida pelo `bff` (GraphQL, app) e pelo `backoffice-bff` (backoffice). Contribuições recentes do Tobias concentradas no content-type `agreement` (assinaturas/contratos).
