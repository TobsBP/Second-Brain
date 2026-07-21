---
type: project-overview
code: FinAI
title: Fin AI — assistente financeiro conversacional
classes_ingested: 1
---

# Fin AI

App mobile de finanças pessoais conversacional: assistente de IA por chat, insights financeiros, perfil e histórico de transações.

## Stack
Expo Router v57 + React 19 + React Native 0.86, NativeWind v4. Alias `@/*` → `src/*`.

## Estrutura
- `src/app/`: rotas puras — `(auth)/{login, register}`, `(tabs)/{chat, insights, profile}`, `(tabs)/history/` com Stack aninhado (lista + detalhe da transação, mantendo a tab bar visível).
- `src/modules/<feature>/`: `screens/` (implementação real), `services/` (chamadas via `src/lib/api/client`, validação Zod), `hooks/` (`@tanstack/react-query`), `store/` (Context só quando o estado precisa ser global), `types/` (schemas Zod).
- Tokens de design vêm de `stitch_fin_ai_conversational_finance/precision_finance_logic/DESIGN.md`.

Não há repositório de backend próprio localizado — consome uma API externa via `EXPO_PUBLIC_API_URL`.

## Sources
- [[raw/projects/FinAI/Nota.md]]
