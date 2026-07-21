Levantado via GitHub API (`Mulher-em-Forma/ticket-support`) em 2026-07-21.

## README
"Formulário de abertura de chamados de suporte da MEF. Coleta dados do usuário (nome, e-mail, CPF, WhatsApp), tipo de problema, descrição e anexos, envia arquivos e cria o chamado em uma API externa."

### Stack
Next.js 16 (App Router) + React 19, Tailwind CSS 4 + shadcn/ui (Radix), Biome, TypeScript.

### Variáveis de ambiente
`NEXT_PUBLIC_API_URL` — URL base da API de suporte (endpoints `POST /api/upload` e `POST /support`).

## Relação
É o formulário de entrada que alimenta a API `class-ticket` (classificação por IA) e, dali, o backoffice de CS.
