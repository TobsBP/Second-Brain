Levantamento a partir de `~/Documents/projects/fin-ai/`.

## O que é
App mobile de finanças pessoais conversacional — um assistente de IA por chat, com insights financeiros, perfil e histórico de transações.

## Stack
Expo Router v57 + React 19 + React Native 0.86, estilizado com NativeWind v4 (Tailwind para RN). Alias `@/*` → `src/*`.

## Estrutura
- `src/app/`: rotas puras (sem lógica de negócio) — `(auth)/{login,register}`, `(tabs)/{chat, insights, profile}`, `(tabs)/history/` com seu próprio Stack aninhado (lista + `[id]` de detalhe da transação, mantendo a tab bar visível ao empurrar para o detalhe).
- `src/modules/<feature>/`: `screens/` (implementação real, a rota só re-exporta), `services/` (funções que chamam `api` de `src/lib/api/client` e validam com Zod), `hooks/` (`@tanstack/react-query`), `store/` (Context, só quando o estado precisa ser global — ex.: auth), `types/` (schemas Zod com `z.infer<>`, sem pasta `validations/` separada).
- `src/lib/api/client.ts`: wrapper de fetch com base URL de `EXPO_PUBLIC_API_URL`, bearer token, `ApiError`.
- Tokens de design (cores, escala tipográfica, raios, espaçamento) vêm de `stitch_fin_ai_conversational_finance/precision_finance_logic/DESIGN.md`, plugados no `tailwind.config.js`.

## Observação
Não há repositório de backend próprio identificado na pasta `projects/` — o app consome uma API via `EXPO_PUBLIC_API_URL` (endpoint não documentado localmente).
