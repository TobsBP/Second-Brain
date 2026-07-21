Levantamento a partir de `~/Documents/projects/start/`.

## O que é
Site + painel administrativo para uma equipe estudantil de robótica/automação ("RoboBulls"), aparentemente ligada a um curso técnico (seções "home-mec-stamp", "home-disciplines", "home-other-courses" sugerem vínculo com um curso de Mecânica/Automação).

## Instrução original (AGENTS.md)
- Deve ter services, types (Zod) e hooks; queries via TanStack Query; Tailwind, Biome, Husky + lint-staged.
- Upload de planilha com nome dos alunos, definindo o time de cada aluno.
- Página para gerenciar o "chaveamento" (keying) de partidas, criando o bracket a partir do número de times.
- Código limpo, sem comentários.

## Stack
Next.js (App Router) + Firebase (Auth/Firestore, `src/lib/firebase.ts` e `firebase-admin.ts`) + TanStack Query.

## Estrutura
- `(admin)/`: automation, brackets, calendar, configuracoes, electrical, library, projects, relatorios, software, students.
- `components/automation/`: automation-coordinator, automation-hero, automation-labs, automation-network, automation-robobulls.
- `components/students/`: upload de alunos (`students-upload-zone`), gestão, estatísticas, tabela.
- `components/match-keying.tsx`: chaveamento de partidas.
- `services/`: match-service, student-service, trip-service, user-service.
- `hooks/`: use-matches, use-students, use-trips, use-users, use-auth, use-theme.
- Página pública (`home-*`): hero, disciplinas, marquee de equações, notícias, outros cursos — parece também servir como site institucional/landing além do painel admin.
