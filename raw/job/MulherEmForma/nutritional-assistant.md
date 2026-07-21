Levantado via GitHub API (`Mulher-em-Forma/nutritional-assistant`) em 2026-07-21.

## README
"API do Assistente Nutricional, uma aplicação FastAPI que tem como objetivo auxiliar os usuários na criação de planos alimentares personalizados com base em suas preferências e necessidades calóricas."

### Arquitetura (diagrama mermaid no README)
```
External: BFF, n8n
Nutrition Service (FastAPI): REST API → Routes → Services → Repositories (camadas)
```

## Stack
Python, FastAPI.

## Observação
Contribuições recentes do Tobias concentradas em CI/testes (cobertura, formatação com ruff, organização de mocks), não em features de produto.
