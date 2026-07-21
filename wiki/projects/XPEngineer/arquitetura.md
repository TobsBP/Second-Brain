---
type: project-concept
project: XPEngineer
tags: [architecture, gamification, mobile]
updated: 2026-07-21
---

# Arquitetura do XP Engineer

| Serviço | Stack | Papel |
|---|---|---|
| **XP-Engineer-API** | Fastify + TS + PostgreSQL, Cloudflare R2 (PDFs), Zod | Usuários, lições, conquistas, progresso, streaks, progressão de nível, listas de exercícios |
| **easy-eng** | Expo Router (mobile), NativeWind, React Navigation, TanStack Query, react-native-katex | App do aluno — trilha de aprendizado, XP, fórmulas matemáticas renderizadas |
| **xp-engineer-admin** | Next.js 16 + React 19 + Tailwind v4 + shadcn/ui (Base UI), TanStack Query, React Hook Form + Zod, KaTeX | Administradores criam/mantêm módulos, lições, quizzes e listas de exercícios consumidos pelo `easy-eng` |

Mesmo padrão de app+admin+API já visto em [[wiki/projects/Fetin/arquitetura]] (OneFit): uma API central consumida por um app mobile do usuário final e um painel administrativo separado para gestão de conteúdo.

## Sources
- [[raw/projects/XPEngineer/Arquitetura.md]]
