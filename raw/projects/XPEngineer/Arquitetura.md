Levantamento a partir de `~/Documents/projects/xp-engineer/{XP-Engineer-API, easy-eng, xp-engineer-admin}`.

## O que é
Plataforma gamificada de aprendizado de engenharia, estética "Precision-Play" (inspirada em consoles de diagnóstico e esquemáticos técnicos). Caminho de aprendizado dinâmico (da matemática básica à integridade estrutural), sistema de XP e progressão por nível, perfil especializado.

## XP-Engineer-API
- Fastify + TypeScript + PostgreSQL (`pg`), `@fastify/jwt`, `@fastify/cors`, `@fastify/swagger` + Scalar.
- Validação com Zod (`fastify-type-provider-zod`).
- Armazenamento de PDFs (listas de exercício) em Cloudflare R2 (S3-compatível).
- Módulos: usuários, lições, conquistas (achievements), progresso, streaks, progressão de nível, listas de exercícios.

## easy-eng (app do aluno)
- Expo Router (mobile), React Native, NativeWind.
- `@react-navigation/bottom-tabs`, `@tanstack/react-query`, `axios`, `dayjs`.
- `react-native-katex` para renderização de fórmulas matemáticas.
- `@sentry/react-native` para observabilidade.

## xp-engineer-admin
- Next.js 16 (App Router) + React 19 + Tailwind v4 + shadcn/ui (sobre Base UI).
- TanStack Query (estado de servidor), React Hook Form + Zod (formulários), Axios com interceptor de token JWT.
- KaTeX para renderização de fórmulas.
- Administradores criam/mantêm módulos, lições, quizzes e listas de exercícios consumidos pelo app do aluno (`easy-eng`).

## Relação entre os três
`XP-Engineer-API` é o backend único consumido tanto por `easy-eng` (aluno, mobile) quanto por `xp-engineer-admin` (administração de conteúdo, web) — mesmo padrão de app+admin+API dos outros projetos (ex.: [[raw/projects/Fetin/Arquitetura.md|Fetin/OneFit]]).
