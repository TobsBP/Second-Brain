Levantado via GitHub API (`Mulher-em-Forma/strapi-migrator`) em 2026-07-21.

## README
"Strapi v4 → v5 Migrator — ferramenta completa, determinística e reexecutável para migração de dados do Strapi v4 para Strapi v5, preservando 100% da integridade relacional."

### Características
Determinística (mesma entrada → mesma saída), reexecutável, segura (nunca escreve no banco v4, só leitura), logada, validável (relatórios de integridade), batch processing, dry run, mapeamento de IDs persistido, preserva relações 1:N e N:N.

### Requisitos
Node.js ≥18, PostgreSQL (Strapi v4), Strapi v5 em execução com API acessível, token de API.

## Relação
Usado para migrar `mef-content-api-v2` (upgrade Strapi v4→v5) e possivelmente outras instâncias Strapi da MEF.
