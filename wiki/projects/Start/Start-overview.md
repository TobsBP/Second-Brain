---
type: project-overview
code: Start
title: Start — painel da equipe RoboBulls
classes_ingested: 1
---

# Start

Site + painel administrativo para uma equipe estudantil de robótica/automação ("RoboBulls"), aparentemente ligada a um curso técnico de Mecânica/Automação (seções de disciplinas e outros cursos na home).

## Escopo original (AGENTS.md)
- Upload de planilha com nome dos alunos, atribuindo o time de cada um.
- Página de "chaveamento" (keying) de partidas — cria o bracket a partir do número de times.
- Services, types (Zod) e hooks; queries via TanStack Query.

## Stack
Next.js (App Router) + Firebase (Auth/Firestore) + TanStack Query.

## Estrutura
`(admin)/`: automation, brackets, calendar, configuracoes, electrical, library, projects, relatorios, software, students. Componentes de automação (`automation-robobulls`, `automation-network`, `automation-labs`), gestão de alunos com upload em massa, chaveamento de partidas (`match-keying.tsx`), viagens (`trip-service`).

## Sources
- [[raw/projects/Start/Nota.md]]
