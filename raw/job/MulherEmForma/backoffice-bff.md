Levantado via GitHub API (`Mulher-em-Forma/backoffice-bff`) em 2026-07-21.

## README
"BFF (Backend for Frontend) para o backoffice da MEF." Composto por vários módulos, cada um com suas próprias responsabilidades e endpoints:

- **Assistant** — gerencia mensagens do assistente nutricional (busca com paginação/filtro por favoritas, exclusão de mensagem).
- **Amplitude** — busca dados de usuário e atividades na plataforma Amplitude.
- **Audit** — registra e busca eventos de auditoria (filtros por ator, entidade, usuário, tipo, tags).
- **Menu** — gerencia o cardápio dos usuários.
- (lista truncada na leitura — outros módulos não confirmados)

## Relação
É o BFF que alimenta o frontend `mef-backoffice-cs`, junto com `mef-analytics-backoffice` e `class-ticket` (ver [[wiki/job/MulherEmForma/MulherEmForma-overview]]).
